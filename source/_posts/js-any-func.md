---
title: js部分整理
date: 2016-04-17 19:55:34
tags: [javascript]
categories: [前端, javascript]
description:
---

通过js动态设置每一个元素的尺寸
setTimeout() 方法用于在指定的毫秒数后调用函数或计算表达式。
setTimeout(code,millisec)  code：必需。要调用的函数后要执行的 JavaScript 代码串。       millisec：必需。在执行代码前需等待的毫秒数。
提示：setTimeout() 只执行 code 一次。如果要多次调用，请使用 setInterval() 或者让 code 自身再次调用 setTimeout()。
setInterval() 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。
setInterval(code,millisec[,"lang"]) code：必需。要调用的函数或要执行的代码串。millisec：必须。周期性执行或调用 code 之间的时间间隔，以毫秒计。
返回值
一个可以传递给 Window.clearInterval() 从而取消对 code 的周期性执行的值。
例：var int=self.setInterval("clock()",50)
<button onclick="int=window.clearInterval(int)">
document.body.clientWidth ==> BODY对象宽度
document.body.clientHeight ==> BODY对象高度
document.documentElement.clientWidth ==> 可见区域宽度
document.documentElement.clientHeight ==> 可见区域高度
documentElement 属性可返回文档的根节点。
document.body是DOM中Document对象里的body节点， document.documentElement是文档对象根节点(html)的引用。
     IE在怪异模型（quick mode）下document.documentElement无法正确取到clietHeight scrollHeight等值，比如clietHeight=0。可以见IE的怪异模型并没有把html作为盒子模型的一部分，好在现在很少使用怪异模 型。（注：如果页面没写DTD或写的不对，IE6默认使用怪异模型解析页面）
document.body.scrollHeight和document.documentElement.scrollHeight的区别：
     document.body.scrollHeight是body元素的滚动高 度，document.documentElement.scrollHeight为页面的滚动高度，且 document.documentElement.scrollHeight在IE和Firefox下还有点小差异。
     IE : document.documentElement.scrollHeight = document.body.scrollHeight + marginTop bottom高度 + 上下border宽度
     firefox : document.documentElement.scrollHeight = document.body.scrollHeight + marginTop bottom高度
这是DOMDocument对象里的body子节点和整个节点树的根节点root。
DOM把层次中的每一个对象都称之为节点，就是一个层次结构，你可以理解为一个树形结构，就像我们的目录一样，一个根目录，根目录下有子目录，子目录下还有子目录。
以HTML超文本标记语言为例：整个文档的一个根就是<html>,在DOM中可以使用document.documentElement来 访问它，它就是整个节点树的根节点。而body是子节点，要访问到body标签，在脚本中应该写：document.body。


