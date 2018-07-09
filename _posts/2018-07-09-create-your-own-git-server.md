---
layout: post
title: 搭建git
comments: true
category: misc
permalink: /:year/:month/:day/:title
tag: misc, tech, git
---

其实如何搭建[官网](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)说的非常清楚啦

###server端搭建repo
首先在server上建立一个新的用户git，这样就只有git的用户能执行git相关操作，会把分工分的很清楚。
```
$ sudo adduser git
$ su git
$ cd
$ mkdir .ssh && chmod 700 .ssh
$ touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```
搭建git repo
```
$ cd /home/git/
$ mkdir project.git
$ cd project.git
$ git init --bare
Initialized empty Git repository in /srv/git/project.git/
```
###用户创建
创建ssh key，一路点默认的话就会把公私钥创建在~/.ssh里
```
ssh-keygen -t rsa -C "your_email@example.com"
```
```
$ cd myproject
$ git init
$ git add .
$ git commit -m 'initial commit'
$ git remote add origin git@gitserver:/home/git/project.git
$ git push origin master
```
这时候发现push的时候会返回没有权限，这时候要把~/.ssh/id_rsa添加到server中 ~/.ssh/authorized_keys里面。
再次push就成功啦。
