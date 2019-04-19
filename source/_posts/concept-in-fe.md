---
title: 编程中的名词
date: 2019-04-19 20:17:18
tags: [前端]
categories:
  - [前端]
description: summary
---

##### 自省
这个概念是学python同学遇到的一个概念。因为在python中一切皆对象，自省表示查看对象的一些特性，推断对象的结构和能力。

##### js中的伪数组
伪数组是一个含有length属性的json对象。像函数中的arguments对象，还有getElementsByTagName,document.childNodes之类的返回NodeLists对象都是伪数组。

##### 浏览器事件模型
这个真的好久好久没见过，导致我忘的一干二净。事件发生定义成三个阶段：捕获阶段，目标阶段，冒泡阶段。

##### 如何在箭头函数中得到argus
箭头函数是不绑定arguments对象的，那么如何在函数内部获得呢？
```js
// 1
var bar = (...arguments) => console.log(arguments);
// 2
var fapply = (fun, args) => fun(...args);
```
