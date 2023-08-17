---
author: Ahmad Bayhaqi
pubDatetime: 2022-01-22
title: "How to setup simple nightly build with Github Actions"
postSlug: nightly-build-github-actions
featured: false
draft: false
tags:
  - actions
  - github
  - docker
ogImage: ""
description: ""
---

## Table of Contents

## Intro

Github actions provide us with many features we can use, simple, and integrated
with any platform. one useful feature is scheduling workflow. Github actions
allow us to run workflow with cron scheduling. this will be useful when we want
to create a scheduled workflow, for example here nightly build. here I want to
share my experience "How to setup nightly build with GitHub actions."

![thumb](https://user-images.githubusercontent.com/56214296/150106740-9574ff31-c90f-49fc-a273-33ec10bc5a21.png)

### setup registry

here we would build the application into a docker image and push it to Github
Container Registry. see here how to push docker image into docker hub.

the first thing we should create Github PAT (Personal Access Token) that allows
us to give specific permission access to our Github account without an actual
password. To generate Github PAT go to `Settings > Developer settings > Personal
access token` and make sure you check this out.

![select scopes](/assets/images/select-scopes.png)

for security purposes, I recommend you to pay attention to the least privilege
principle. please give your PAT only the necessary scope. see helpful official
documentation about Github PAT [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

after that store your Github PAT to Github secret to make sure all are encrypted
securely. see [how to create Github secrets.](https://docs.github.com/en/actions/security-guides/encrypted-secrets)

### Setup GitHub actions

this setup is very simple just build an image and push the image to the registry.
this workflow uses official docker Github action. buildx and docker login.

<script src="https://gist.github.com/Kyuubang/530320bc9e4732f497a5bae43d23a98f.js"></script>

In the first step, we should checkout the repository. next, we use buildx to
build docker instance. This action will create and boot a builder that can be
used in the following steps of your workflow if you're using [buildx](https://github.com/docker/buildx).
By default, the `docker-container` builder driver will be used to be able to
build multi-platform images and export cache thanks to the BuildKit container
third step, use docker login action to authenticate with Container Registry,
here we use Github Container Registry. we should define with registry parameter
with ghcr.io value. finally, build an image and push it into the registry. you
can define a custom Dockerfile path or name.

![docker image nightly](/assets/images/docker-nightly.png)

you can check the container will be built nightly. to show your docker image go
to profile > package. by default, the workflow would be run in UTC time zone.
you should adapt to your time zone. for example, I'm in WIB Time Zone or equal
to UTC+7 and I can write the cron-like this

```yaml
name: "Nightly Build"

on:
  schedule:
    - cron: "0 7 * * *"
```

sometime cron syntax quite confusing, i very recommended to use this [website](https://crontab.guru/)
to create your cron.
