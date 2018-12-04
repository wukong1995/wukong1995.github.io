---
date: 2016-12-11 18:30:42
title: moment格式化时间
tags: ['nodejs']
categories: ['nodejs']
---
首先，好久没有来多博客了

今天在学习MongoDB时，需要格式化时间，上网查了一下，nodejs中的moment模块可以格式化时间。
首先，mongodb中有一个字段是Date类型需要一个默认值，就是当前时间，可以使用以下代码来格式化时间

```javascript
moment().format('YYYY-MM-DD HH:mm:ss')  
```
使用以下代码也是可以的

```javascript
moment(Date.now()).format('YYYY-MM-DD HH:mm:ss')  
```
目前只使用这两个方法，以后再来补充
------------------------------------------------------分割线----------------------------------
在存入[数据库](http://lib.csdn.net/base/mysql)时，我已经格式化日期了，取出来发现是没有格式的数据，很纳闷，不知道原因，找到了解决办法。
express我是用的jade模板，
在app.js中加入

```javascript
app.locals.moment = require('moment')  
```
在jade中读取日期数据使用

```javascript
#{moment(friend.createTime).format('YYYY-MM-DD HH:mm:ss')}  
```
取出来的日期是格式化的
