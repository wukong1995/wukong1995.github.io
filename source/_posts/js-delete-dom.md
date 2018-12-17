---
title: javaScript删除元素
date: 2016-06-03 11:15:46
tags: [javascript]
categories: [前端, javascript]
description:
---


有一段html代码
```html
<div>
    <div id="div1">div1</div>
    <div id="div2">div2</div>
    <div id="div3">div3</div>
    <div id="div4">div4</div>
    <div id="div5">div5</div>
</div>
```

假如我想删除div中的第二个div，我需要找到这个元素，在找到父元素，用removeChild进行删除。
但是，看到一个办法，就是使用 outerHTML = '' 这个方法，outerHTML可以获得含标签在内的字符串，将字符串置空，div就消失了。

不知道这个方法有什么后遗症，待实际使用后，再来补充。


PS：所有的测试都是在谷歌浏览器上进行的


