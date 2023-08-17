---
author: Ahmad Bayhaqi
pubDatetime: 2022-06-10
title: "Lab Setup Ansible single node with virtualenv on Ubuntu 18.04"
postSlug: single-node-ansible-ubuntu-18-04
featured: false
draft: false
tags:
  - ansible
ogImage: ""
description: ""
---

this guide will share how to me create a lab setup to learn ansible with single
machine. start with preparing requirements, installing, and run sample playbook.
installing method using virtualenv. see how to create lab setup use package manager.

### Preparing Environment

Ansible writen in python, so python is main requirement to have. in official documentation
python older than python 3.8 no longer supported to run ansible. we need python 3.8 above
to ran ansible properly.

check current python version

```bash
$ python --version
```

if python not installed, use this command and customize with your available version on your distro.
and also we need pip too.

```bash
$ sudo apt install python3.8 python3-pip
```

> **protip**
> after installing python3.8<= you might needed to link default python executable. to link use this command.
>
> ```bash
> $ sudo ln -s /usr/bin/python3.8 /usr/bin/python
> ```

install virtualenv with pip

```bash
$ python -m pip install virtualenv
```

after that create virtual environment virtualenv, and activate it.

```bash
$ virtualenv belajar-ansible; source belajar-ansible/bin/activate
```

if your shell changed like `(belajar-ansible)kyuubang@mylinux:~$` your virtualenv
already to use.

### Installing ansible with pip

next, install ansible to virtual environment with pip.

```bash
(belajar-ansible)$ pip install ansible
```

### create ansible inventory

if no error found, next we can next to use ansible. global inventory ansible found
in `/etc/ansible/hosts`.

```bash
(belajar-ansible)$ sudo mkdir /etc/ansible; sudo vim /etc/ansible/hosts
```

in this case we'll you single node as lab, in other word we just use localhost as target host.

```bash
[master]
controller ansible_host=127.0.0.1 ansible_user=ansible
```

### preparing connection

ansible use ssh bydefault to connect into host target. we optional recommended
to setup ssh-key, and config sudoers.

create ssh-key with ssh-keygen

```bash
(belajar-ansible)$ ssh-keygen
```

just enter to finisih key gen, after that copy public key to `authorized_keys`

```bash
(belajar-ansible)$ cat .ssh/id_rsa.pub >> authorized_keys
```

try to connect into localhost over ssh. after that, you can try to test ping with ansible
in order to make sure inventory are reachable.

```bash
(belajar-ansible)$ ansible all -m ping
controller | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### sample playbook

try to use sample ansible playbook, in this case we'll tried to install nginx and copy
index.html into target host. ansible use yaml as playbook format.

```yaml
- hosts: "minion"
  become: yes

  tasks:
    - name: install nginx
      apt:
        name:
          - nginx
        state: present
    - name: copy index.html
      ansible.builtin.copy:
        src: ~/data/index.html
        dest: /var/www/html/index.html
```

to check all syntax has no error, use this command.

```bash
(belajar-ansible)$ ansible-playbook --syntax-check <name_playbook>.yml
```

if everythin is ok, and no error found. you can run ansible playbook with `ansible-playbook`

```bash
(belajar-ansible)$ ansible-playbook <name_playbook>.yml
```

this sample successfully output with run ansible.

![Simple output success playbook](/assets/images/ansible-sample-output.png)
