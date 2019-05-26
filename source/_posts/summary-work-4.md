---
title: 这周也是元气满满的一周
date: 2019-05-25 12:11:26
tags: [react]
categories: [前端, react]
description: 适应了比之前疲劳的一周～
---

1. PureComponent为什么不管用了
首先这个问题可以追溯到我之前的[issue](https://github.com/apollographql/react-apollo/issues/2380)，这次拿出来看的时候，终于是找到了清楚的答案。

使用PureComponent可以提高性能，因为react会在shouldeComponentUpdate中进行一个浅比较，注意是浅比较，所以当你的UI组件中一旦有嵌套的对象属性或者函数，其实这个时候即使用了PureComponent也不会有什么作用，这个时候就需要你在shouldeComponentUpdate去进行手动判断是否rerender。

[参考链接：你真的了解浅比较么？](https://imweb.io/topic/598973c2c72aa8db35d2e291)

2. ssr
当你访问一个spa时，第一次打开的速度很慢，白屏的时间也很长，打开控制台发现用了css in js导致html巨大，js文件也很巨大，这个时候最简单的是使用部分ssr，先显示一部分html在页面，然后等下载完js文件后执行即可。

3. 项目里面用了css in js，想要把它干掉，不知道执行中会遇到什么问题，这是自己的一个挑战吧。

4. 因为现在工作时间变长了，真的是很难调节自己的时间，每天回到家只想躺着，努力调整一下。