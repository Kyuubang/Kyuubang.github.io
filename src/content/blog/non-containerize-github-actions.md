---
author: Ahmad Bayhaqi
pubDatetime: 2022-03-11
title: "simple non-containerize application with GitHub actions"
postSlug: non-containerize-cicd
featured: false
draft: false
tags:
  - actions
  - github
  - bash
ogImage: ""
description: ""
---

## Table of contents

## Intro

I worked on many projects with Github as DevOps in my team. I have a responsibility
to deliver applications from development to production. At first, I did it manually.
I started to feel this is always reprising every time there is a change in the application.

![thumb](https://user-images.githubusercontent.com/56214296/158066250-935a0d01-93be-4215-8472-e93762292483.png)

Our production environment still uses a bare metal server. The application ran
on VMs created in proxmox. Production ran a simple stack. but if a change or
feature is added to the Github repository my dev friends say, "Hey I added new
feature, can you deploy it?" and it keeps repeating. until I found solution to
create Github actions that can continuously check and deliver the application.

### provisioning DB

Our application depends on PostgreSQL. I used containerize DB because it's simple
to provisioning and you know.. I want to keep the server clean. to realize it
creates a docker-compose file to automate provisioning.

<script src="https://gist.github.com/Kyuubang/ccf6698eebf06791d766d48178b6cdb4.js"></script>

let's make this up in daemon mode.

```bash
docker-compose -f docker-compose.db.yml up -d
```

### Create deploy.sh

to automate working my repeating task in deployment, I use simple bash scripting.  
this script would be run every deployment.

<script src="https://gist.github.com/Kyuubang/65e32082726bdf335458461d0ea2db17.js"></script>

the working directory would be stashing before pulling the repository, this is
done so that there is no conflict merge. and then did pulling repo.
in production have little change configuration settings like DB. I use sed to
change the settings file with one line. **make sure already migrated**. it always
restarts service gunicorn and nginx to minimalization failure. see how to create
gunicorn.service file.

### Setup Github actions workflow

The first thing we should do is create GitHub secrets. Github secrets are encrypted
secrets allowing us to store sensitive information in your organization, repository,
or repository environments. learn more here.

```text
SSH_KEY         -> ssh private key (for server connection)
SSH_PORT        -> ssh port
SSH_PASSPHRASE  -> optional but highly recommended
SSH_HOST        -> host ssh server (target server)
```

may you ask "Why Host and Port should be in github secrets?" i recommend to
keep all data of server is encyrpted and secret for security purpose. next,
what I did is set up Github actions to trigger change and run deploy.sh
over ssh. here are my Github actions

<script src="https://gist.github.com/Kyuubang/1f6590e2d95c4e7ffb62414288f82b6d.js"></script>
