---
author: Ahmad Bayhaqi
pubDatetime: 2022-10-03
title: "How to recreate MONs out of quorum on Ceph"
postSlug: recreate-ceph-mon
featured: false
draft: false
tags:
  - ceph
  - release
ogImage: ""
description: trouble shooting your ceph mon and recreate it
---

## Table of contents

## Intro

MONs are recommended to ensure fault tolerance and maintainability. If a cluster's MONs cannot form a quorum, which happens if not enough of the provisioned MONs are up, then new clients won't be able to connect to the system.

quorum is fundamental to all consensus algorithms that are designed to
access information in a fault-tolerant distributed system. Minimum number of votes achieve consensus among a set of nodes called quorum.

Sometime you had Ceph MONs are down, and return `out of quorum` status. This tutorial you will learn How to recreate MON daemon on Ceph.

![preview](https://user-images.githubusercontent.com/56214296/194529209-18af5fd9-73a9-4ed4-9a1d-77d3604ef7b0.png)

### Step 1 -- Remove MONs

first step you need to see the status of cluster. based on the information below, you have 1/3 mons down, and return `out of quorum` status and everything else is safe.

```bash
$ sudo ceph status
  cluster:
    id:     4664a822-3fac-11ed-b14c-5bcb09093cfa
    health: HEALTH_WARN
            1/3 mons down, quorum pod-bayhaqisptr04-ceph1,pod-bayhaqisptr04-ceph2

  services:
    mon: 3 daemons, quorum pod-bayhaqisptr04-ceph1,pod-bayhaqisptr04-ceph2 (age 0.343641s), out of quorum: pod-bayhaqisptr04-ceph3
    mgr: pod-bayhaqisptr04-ceph1.ufgzyk(active, since 3d), standbys: pod-bayhaqisptr04-ceph2.qcmvuo
    osd: 6 osds: 6 up (since 117m), 6 in (since 117m)

  data:
    pools:   1 pools, 1 pgs
    objects: 2 objects, 449 KiB
    usage:   105 MiB used, 30 GiB / 30 GiB avail
    pgs:     1 active+clean
```

you should remove mon from the ceph cluster. you can type with command below.

```bash
sudo ceph mon remove pod-bayhaqisptr04-ceph3
```

> **Info**
>
> ID of MONs is ussually the host name that are provisioned daemon.

after successful remove mon from cluster you need check again health. The Health will be OK. Because no one MONs down, you already remove it. health are OK but MON decrease 2 daemon in cluster

```bash
sudo ceph status
  cluster:
    id:     4664a822-3fac-11ed-b14c-5bcb09093cfa
    health: HEALTH_OK

  services:
    mon: 2 daemons, quorum pod-bayhaqisptr04-ceph1,pod-bayhaqisptr04-ceph2 (age 3s)
    mgr: pod-bayhaqisptr04-ceph1.ufgzyk(active, since 3d), standbys: pod-bayhaqisptr04-ceph2.qcmvuo
    osd: 6 osds: 6 up (since 2h), 6 in (since 2h)

  data:
    pools:   1 pools, 1 pgs
    objects: 2 objects, 449 KiB
    usage:   105 MiB used, 30 GiB / 30 GiB avail
    pgs:     1 active+clean
```

### Step 2 -- Make sure daemon are removed on Node

MONs might removed from cluster, but what about the Node itself? you need to checkout to host.

```bash
$ ssh ceph3
$ sudo cephadm ls | grep mon
        "name": "mon.pod-bayhaqisptr04-ceph3",
        "systemd_unit": "ceph-4664a822-3fac-11ed-b14c-5bcb09093cfa@mon.pod-bayhaqisptr04-ceph3",
        "service_name": "mon",
```

if you see like this output that means daemon was stopped but daemon still registered on cephadm host. to remove it you can use rm-daemon subcommand on cephadm.

```bash
sudo cephadm rm-daemon --name mon.pod-bayhaqisptr04-ceph3 --fsid 35de853b-3a4e-4568-b698-f223c8382bb8 --force
```

after success remove, check again that are still available or not.

```bash
sudo cephadm ls | grep mon
```

to get your fsid use this command to grep you cluster id

```bash
sudo ceph status | awk '/id:/ {print $2}'
```

it might take a few minutes to update entire database of cephadm before you can redeploy new Ceph MON

### Step 3 -- Redeploy Ceph MON

after daemon are removed you need to redeploy daemon. to reliaze it you can set mon to unmanaged mode

```bash
sudo ceph orch apply mon --unmanaged
```

and redeploy new daemon with given spesific host and address

```bash
$ sudo ceph orch daemon add mon pod-bayhaqisptr04-ceph3:10.9.9.30

Deployed mon.pod-bayhaqisptr04-ceph3 on host 'pod-bayhaqisptr04-ceph3'
```

verify its work properly

```bash
$ sudo ceph status

student@pod-bayhaqisptr04-ceph1:~$ sudo ceph status
  cluster:
    id:     4664a822-3fac-11ed-b14c-5bcb09093cfa
    health: HEALTH_OK

  services:
    mon: 3 daemons, quorum pod-bayhaqisptr04-ceph1,pod-bayhaqisptr04-ceph2,pod-bayhaqisptr04-ceph3 (age 55s)
    mgr: pod-bayhaqisptr04-ceph1.ufgzyk(active, since 3d), standbys: pod-bayhaqisptr04-ceph2.qcmvuo
    osd: 6 osds: 6 up (since 2h), 6 in (since 2h)

  data:
    pools:   1 pools, 1 pgs
    objects: 2 objects, 449 KiB
    usage:   105 MiB used, 30 GiB / 30 GiB avail
    pgs:     1 active+clean
```

Congrats! you have recreate and bringing up you Ceph MON.

## Conclusion

The ceph cluster uses several daemons to build its system. and registered on cluster and host itself. ceph mon is responsible for ensuring all daemons are in good working order.

This guide cover some common cephadm command to operate remove daemon `cephadm rm-daemon`, and also learn how to recreate it `ceph orch daemon add`. the link below might be as useful reference

- https://docs.ceph.com/en/quincy/cephadm/services/mon/#deploying-monitors-on-a-particular-network
- https://stackoverflow.com/questions/69488109/how-to-restart-a-mon-in-a-ceph-cluster
