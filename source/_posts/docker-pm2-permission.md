---
title: pm2在docker上以非root权限启动不了
date: 2020-02-20 13:36:47
tags: [docker]
categories: [docker]
description: 不同机器上不一样的表现==
---

在docker-compose中，我是用works身份去启动去启动docker，在好几台机器上部署了，但是有的机器上可以正常启动，有的机器却显示权限有问题，具体错误是：`docker Error: EACCES, permission denied '/root/.pm2’`

解决办法：
1. ❌在启动pm2的时候指定用户， `pm2 start -u works`，这个方法不可行，仍旧是这个错误
2. ✅在dockerfile中设置`PM2_HOME`

```
WORKDIR /apps

# 设置成工作目录
ENV PM2_HOME="/apps/.pm2"
```