---
title: 服务器-初探
date: 2018-11-20 19:01:00
tags: ['linux', 'centos']
categories: ['server']
description: 服务器-初探
---

#### 目前的想法

需要了解一下服务器的端的知识，才可以在使用过程中不崩溃。
使用中最让我崩溃的是，我在启动pg的时候，使用root身份启动，告诉我不可以使用root身份，使用普通用户启动，告诉我没有文件的读写权限😵

虽然还是没有安装上pg这个gem


#### 1. 新建用户
1. 新建用户，创建密码
```js
# adduser tony
```
2. 赋予root权限
找到三个方法：
2.1. 修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行
```js
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
tommy   ALL=(ALL)     ALL
```
2.2. 修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉
```js
## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)    ALL
```
修改用户，使其属于root组:
```js
# usermod -g root tony
```
2.3. 修改 /etc/passwd 文件，找到如下行，把用户ID修改为 0
```js
tony:x:0:33:tony:/data/webroot:/bin/bash
```

3. 上传ssh-key，关闭密码登录
```js
# ssh-copy-id tony@服务器ip
```
编辑/etc/ssh/sshd_config
```js
#PasswordAuthentication yes 改为
PasswordAuthentication no
```

#### 2. 安装软件
不同系统管理软件的软件不同，对于这个我也是乱乱的，找了一圈，才发现是使用yum安装的....

#### 3. 启动服务
启动的服务的命令（是这样叫吗，或者启动服务的包）大部分在/etc/init.d/文件夹下....

