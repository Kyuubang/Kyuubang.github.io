---
title : "Repository management in openSUSE"
show_date: true
toc: true
tags:
    - zypper
	- repository
categories:
    - linux
---

Repository is a location from where the Linux system retrieves and installs updates
and application related to the Operating system. We can easly manage package update,
update selection, OS updates, etc. openSUSE have 7 category of repository, OSS(open
source software), Non-OSS(non-free as in freedom software), Update(official security
and bugfix updates for OSS packages), Update Non-OSS(official security and bugfix
for Non-OSS packages), Src-OSS(sources RPMs for OSS packages), Src-Non-OSS(Source RPMs
for Non-OSS packages), Debug(debug info packages).

## Intorduction

we know zypper is one of command line tool package manager for SUSE family, included 
openSUSE. I've been using this Operating System for 1 month. I like about 
zypper is output with tabling style, we can easly find a package with just keyword, 
and another one zypper provide us shortcut for a few command. zypper also give us
better experience in Repository management, you can disable, add, remove, aliases 
repository without edit file configuration. 

### List repository 

To view list repository available in your openSUSE run following command.

```bash
~> zypper repos

```
and also we can use shortcut with `lr` stand for `list repo` just run following 
command.

```bash
~> zypper lr
```

#### Understand field of repository openSUSE


### Modify repository

#### change name and alias repository
#### disable and enable repository
#### autorefresh repository

### Add and Remove repository

### update repository


