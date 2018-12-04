---
date: 2018-11-10 19:40:00
title: 再次尝试pm2-自动化部署
tags: [nodejs]
categories: [nodejs, pm2]
description: pm2-自动化部署node项目
---

时间可以给人沉淀，现在再解决一年前的问题，比之前省力✌️。
之前尝试过[pm2](https://wukong1995.github.io/2017/09/13/pm2/)，但是以失败告终。

不知到之前为什么会报nvm的错误，这次使用yum安装node，还算顺利。

```javascript
# yum install node

# pm2 deploy ecosystem.json production setup

# pm2 deploy ecosystem.json production
```

过程中，虽然start了，但是还是无法访问，这个时候，我尝试在服务器上使用node start,发现端口被占用，但是pm2显示还是启动了(因为项目是一年前写的，不知道是不是因为版本太低了还是什么原因)。。。更换端口，可以访问了....

之前我是想使用ror，但是pg那个gem死活装不上，就还是转用node+mongodb

### mongodb的安装

```javascript
# yum -y install mongodb-org
# rpm -qa |grep mongodb              // 验证安装
# rpm -ql mongodb-org-server         // 验证安装
# /etc/init.d/mongod start           // 启动服务
# netstat -nltp|grep mongo           // 查看占用端口
# mongo                              // 进入数据库
```
