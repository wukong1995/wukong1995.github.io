---
title: Boolean隐形类型转换
date: 2018-12-17 19:06:45
tags: [javascript]
categories: [前端, javascript]
description: 你在写if语句有没有有过出乎意料的bug？
---

在if类型中，为了使代码看起来好看一些，你往往会使用最简洁的判断，但是其中涉及的隐形类型转换，一不小心就出了bug。

```js
if (count) {
  // ....
}
```
在运行过程中，if代码块死活不执行，仔细想了想，count等于零的时候，是不执行的，这不是我的本意呀，我想着存在着这个值的时候，就执行，所以我应该这样写，才能达到我的目的。
```js
if (count !== undefined) {
  // ....
}
```

这里有一份列表，表中的值类型转换完一定为false
* undefined
* null
* false
* +0, -0, and NaN
* ""

不在上面列表中，一定为true。

思考一下：
```js
var a = new Boolean( false );
var b = new Number( 0 );
var c = new String( "" );
var d = Boolean( a && b && c );

d; // true
```
你会不会感到疑惑？看一下值时候在上面的列表中！记住只要不在列表中的值，类型转换之后，一定为true


这里补充一下
`+c`的意思是将`c`转换成`number`类型，那么`"foo"++"5"`，就是先执行`+"5"`

