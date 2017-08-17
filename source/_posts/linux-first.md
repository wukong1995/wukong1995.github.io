---
title: linux-first
date: 2017-09-13 14:10:13
articleTitle: 得到一个linux服务器要做的事情
tags: ['linux']
categories: ['linux']
description: 得到一个linux服务器首先要做的事
---

对待自己的服务器，想着反正也没人攻击，也就从来没在意过。
前天得到了一个讯号：要好好对待服务器，第一步就是安全性， 你需要这么做

1. 使用root账户登录后，创建一个用户并设置密码，如果有必要，就赋予用户root权限
2. 上传ssh-key，上传成功后，你就不用输入密码登录服务器了
3. 关闭密码登录，打开ssh-key登录
4. 以后就使用你新建的用户登录吧