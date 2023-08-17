---
author: Ahmad Bayhaqi
pubDatetime: 2022-03-27
title: "How to setup controller node ansible"
postSlug: setup-ansible-controller
featured: false
draft: false
tags:
  - ansible
ogImage: ""
description: ""
---

## Table of contents

## Intro

ansible wrote in python, so we need python 2.7 or python 3.8 higher to run ansible
properly. many given methods to install ansible, here I wanna share with you to
setup controller node ansible with pip.

pip is a python package manager, we use pip because is simple and we can keep it
isolated with virtualenv. to install the latest pip type and run the following
command.

```bash
$ sudo apt install python3-pip
```

and we need virtualenv too to create an isolated environment.

```bash
$ pip install virtualenv
```

and let's create a virtual environment with virtualenv and activated it with the
command below.

```bash
$ virtualenv ansible; source ~/ansible/bin/activate
```

next, we can install ansible with pip install

```bash
(ansible)$ pip install ansible
```

if virtualenv activated check ansible has been installed properly with the
following command

```bash
(ansible)$ ansible --version
```

if no error found, congratulations you succeed in installing ansible on the
controller node next, we can try the simple playbook and configure the Ansible
inventory. Inventory is simply explained as a list or group of managed nodes.
for example, we have two managed nodes called worker and master, and we can type
like this.

```yaml
[master]
node-master ansible_host=192.168.2.2 ansible_user=ansible
[worker]
worker-1 ansible_host=192.168.2.3 ansible_user=ansible
worker-2 ansible_host=192.168.2.4 ansible_user=ansible
```

for grouping identified with sign `[nameofgroup]`. pro tips we can also define
an ip host and default user for ssh.

ansible run task on playbook. here an example playbook for installing docker with
ansible.

<script src="https://gist.github.com/Kyuubang/4fd98b59ab811562b9214b90575e29e5.js"></script>

Playbooks are Ansible’s configuration, deployment, and orchestration language.
They can describe a policy you want your remote systems to enforce, or a set of
steps in a general IT process. ansible playbook have 2 basic component, first is
hosts and users setup, you can be spesify spesific node or just call group name
that has been configured in inventory file. Second is tasks list, task would be
executed in order, one at time, agains all machine matched by the host pattern,
before moving on to the next task. tasks contains
[collections](https://docs.ansible.com/ansible/latest/collections/index.html),
and [modules](https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html).

before we run the playbook file, we can check playbook no have file, roles, or
syntax problem. run with the `--syntax-check` flag.

```bash
(ansible)$ ansible-playbook --syntax-check docker.yml
```

if no errors found we can run ansible playbook with following command, add `-i`
for inventory path, by default ansible read `/etc/ansible/hosts`

```bash
(ansible)$ ansible-playbook -i path docker.yml
```
