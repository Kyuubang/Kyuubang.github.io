---
author: Ahmad Bayhaqi
pubDatetime: 2023-04-03
title: "How to Configure SSL Using Cert-Manager with (AGIC) Application Gateway Ingress Controller"
postSlug: cert-manager-with-agic
featured: false
draft: false
tags:
  - azure
  - kubernetes
ogImage: ""
description: ""
---

## Table of contents

## Intro

Kubernetes has a lot of support and open-source tools to contribute to the cloud-native project. one of the scopes is the ingress, k8s has a support ingress, you might concern nginx ingress that a very famous ingress in k8s. azure has ingress to work well with an azure cloud environment, that is Application Gateway. The Application Gateway Ingress Controller allows [Azure Application Gateway](https://azure.microsoft.com/en-us/services/application-gateway/) to be used as the ingress for an [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/) aka AKS cluster.

![banner](https://user-images.githubusercontent.com/56214296/229412442-fc335b05-eb82-4424-8320-314cd8dbd9fd.jpg)

In this guide, you'll learn how to configure Secure HTTP with Cert-Manager and AGIC. Before getting starting you should have the following requirements.

- AKS Cluster
- Azure Subscriptions
- kubectl installed on your local machine

### Step 1 -- Create application gateway

Create public ip to used by application gateway as front end IP.

```bash
az network public-ip create -n myPublicIp -g myResourceGroup --allocation-method Static --sku Standard
```

Create new subnet within AKS vnet.

```bash
az network vnet subnet create -g MyResourceGroup --vnet-name MyVnet -n MySubnet --address-prefixes 10.226.0.0/24
```

Create Application gateway within AKS vnet, with different subnet. this guide you will create application gateway with Tier Standard, which is isn't act as WAF.

```bash
az network application-gateway create -n myApplicationGateway -l eastus -g myResourceGroup --sku Standard_v2 --public-ip-address myPublicIp --vnet-name myVnet --subnet mySubnet --priority 100
```

### Step 2 -- Enable add-ons to AKS

after success create app gateway, you need to enable the application gateway with AKS cluster,

```bash
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id")
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

### Step 2 -- Deploy sample app

Create deployment and service app, you will deploy sample HTML 5 game.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: clumsy-bird
  name: clumsy-bird
spec:
  selector:
    matchLabels:
      app: clumsy-bird
  replicas: 2
  template:
    metadata:
      labels:
        app: clumsy-bird
    spec:
      containers:
        - name: clumsy-bird
          image: bayhaqisptr/clumsy-bird:latest
          ports:
            - containerPort: 8001
---
apiVersion: v1
kind: Service
metadata:
  name: clumsy-service
  labels:
    run: clumsy-service
spec:
  type: ClusterIp
  ports:
    - port: 8001
      protocol: TCP
  selector:
    app: clumsy-bird
```

### Step 3 -- Create Ingress

Create ingress, you might notice the annotations `kubernetes.io/ingress.class` make sure its use `azure/application-gateway` to tell AKS to use application gateway as Ingress.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: clumsy-agic
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  tls:
    - hosts:
        - appgw-test.example.test
      secretName: clumsy-tls
  rules:
    - host: appgw-test.example.test
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: clumsy-service
                port:
                  number: 8001
```

### Step 4 -- Create Issuer (Staging)

Create Issuer staging with Let's Encrypt with cert-manager

```yaml
apiVersion: cert-manager.io/v1
   kind: Issuer
   metadata:
     name: letsencrypt-staging
   spec:
     acme:
       # The ACME server URL
       server: https://acme-staging-v02.api.letsencrypt.org/directory
       # Email address used for ACME registration
       email: user@example.com
       # Name of a secret used to store the ACME account private key
       privateKeySecretRef:
         name: letsencrypt-staging
       # Enable the HTTP-01 challenge provider
       solvers:
       - http01:
           ingress:
             class:  azure/application-gateway
```

### Step 5 -- Create Issuer (Prod)

Create Issuer staging with Let's Encrypt with cert-manager

```yaml
apiVersion: cert-manager.io/v1
   kind: Issuer
   metadata:
     name: letsencrypt-prod
   spec:
     acme:
       # The ACME server URL
       server: https://acme-v02.api.letsencrypt.org/directory
       # Email address used for ACME registration
       email: user@example.com
       # Name of a secret used to store the ACME account private key
       privateKeySecretRef:
         name: letsencrypt-prod
       # Enable the HTTP-01 challenge provider
       solvers:
       - http01:
           ingress:
             class: azure/application-gateway
```

### Step 6 -- Ingress TLS

after success create issuer with cert-manager, you can enable https connection to handle acme challenge.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mokita-agic
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    cert-manager.io/issuer: letsencrypt-prod-agic
    cert-manager.io/acme-challenge-type: http01
    acme.cert-manager.io/http01-edit-in-place: "true"
spec:
  tls:
    - hosts:
        - appgw-test-16769.southeastasia.cloudapp.azure.com
      secretName: mokita-agic
  rules:
    - host: appgw-test-16769.southeastasia.cloudapp.azure.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: mokita-service
                port:
                  number: 80
```

### Conclusion

In Cloud Native world has many project available to explore. for example k8s has many ingress support to use, link nginx ingress, ingress nginx, kong, agic, etc.

This guide cover some step to configure let's encrypt with agic ingress

- https://azure.github.io/application-gateway-kubernetes-ingress/
- https://cert-manager.io/docs/tutorials/acme/nginx-ingress/
