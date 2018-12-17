---
title: javaScript取得当前元素的下一个元素
date: 2016-05-28 14:10:30
tags: [javascript]
categories: [前端, javascript]
description:
---


如何取得当前元素的下一个元素呢？
例如，这有两个div
```html
<div id="wrap1" class="wrap">这是一个div</div>
<div class="wrap" style="margin-top: 20px">这是一个div</div>
```

我可以取得第一个div
我想取得紧邻它的下一个元素，从网上获取的方法是：div1.nextSibling，然会我会得到一个#text

可是我想要的不是这个东西，我想得到像变量div1一样的div,经过测试下面两种方法都可以
这样就得到了我想得到的下一个元素了
如果某元素的紧邻的下一个元素不存在，则返回一个 null

注：只在谷歌浏览器下测试


