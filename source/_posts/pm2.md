---
date: 2017-09-13 21:29:01
title: node项目自动化部署
tags: ['nodejs']
categories: ['nodejs']
description: 自动化部署node项目
---

#### 由来
嘤嘤，看到`ruby`中有一个`gem`叫做`capistrano`， 使用它可以实现一键部署到服务器上了。
之前在懵里懵懂的时候，做了一个网站，每次更改后，我都用`filezilla`将新版本的代码上传，再restart项目，超级费劲。
看到ruby后，于是赶紧去网上搜了搜，`pm2`可以做这件事。之前也见到过`pm2`，那时候我在寻求守护进程的module，但是我选择了`forever`。

#### 使用pm2实现自动部署
[这个链接对我的帮助很大](https://github.com/i5ting/nodejs-fullstack/blob/master/deploy.md)，基本上按照他的步骤来应该是可以实现自动化部署。
##### 出现的问题
服务器上的node是我很久之前安装的，但是`pm2`要求使用`nvm`，于是装了`nvm`，但是到了“克隆好之后执行安装和启动”，这一步报错：npm 这个命令找不到。去寻找解决办法，有的说将`.bashrc`文件中的关于`nvm`的部分放在最上面，还有的说要使用nvm重新安装node，这两个方法都试了，但是都没有解决错误。于是我只能去`pm2`的目录中的source文件夹，手动去执行`npm install` 和 `pm2 start app.js`

等待update得到解决方案：

#### nginx转发
最暴力的方法就是下面👇的代码：
```
server{
    listen 80;
    server_name xxx.xxx.xxx;(域名)
    location / {
        proxy_pass http://127.0.0.1:3000
    }
}
```
目前可以解决我的需要，假如还有其他的端口需要监听该怎么办？
