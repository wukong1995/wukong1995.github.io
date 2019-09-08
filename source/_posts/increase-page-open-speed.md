---
title: 提高页面打开速度-优化打包
date: 2019-09-01 18:52:53
tags: [前端, webpack]
categories: [webpack]
description: 调整webpack的打包方式
---

### cacheGroups
webpack4新有的一个cacheGroups， 现在我看大部分人都倾向于把node_modules里面的打成一个文件，像下面这样：
```js
vendor: {
  test: /[\\/]node_modules[\\/]/,
  name: 'vendor',
  chunks: 'all',
  minSize: 1,
  minChunks: 1,
  priority: 10,
  reuseExistingChunk: true
}
```
但是比较尴尬的是，项目时使用spa写的，如果这样做的话，vendor文件会巨大无比，现在是打包压缩完是4M，非常庞大的文件。

仔细看了webpack的分析，类似echarts这样的大包也会打进去，但是echarts只在一个页面中使用了，如果一打开页面，就开始加载，太昂贵了。于是我调整了一下打包的策略，将“minChunks”改成2，最少两次引用的才会被打进vendor文件，这样导致的结果是有些页面的体验不是特别好，于是再为这些页面想想办法。

### externals
项目已经使用了http2，这样带来的好处是我可以使劲将文件切分，越小越好，像上面的问题，我只能在一打开页面的时候就去请求文件，这样的好处是等打开页面的时候，加载更快，体验更好。想了想，还是将文件放在cdn上，在webpack里面加入externals

```js
externals: {
  jsPlumb: 'jsPlumb'
}
```
放在cdn上的好处是，只要用户访问过一次，就缓存了，即使经历迭代上线，也不会再去请求更新。

### 提取公共包？
现在项目是有两个repo组成的，技术栈一模一样，上面的方法带来的启示是，我要不要将两个项目里面的包提取出来，放成一个cdn，这样既节省了打包速度，又减少了一次很大的包请求？

### 引入iconfont
项目现在还没有使用iconfont，全都是用的图片，真的是非常想用iconfont代替它，因为使用图片有太多的局限，使用起来也很麻烦，但是实在是没有想到一个好办法来解决这和事情，真的是好纠结。说到这又想起来一个问题，之前尝试将iconfont.css加入到项目，但是webpack打包老是不过，现在打包是这样设置的：
```js
{
  test: /\.css$/,
  include: /node_modules/,
  use: isClient ? [MiniCssPlugin.loader, 'css-loader'] : 'css-loader/locals'
}
```
真的是很对不起自己，没有看过webpack的文档，于是代入式的认为`include`表示的意思是在开发目录的基础上在包括的意思。在多次尝试无果的基础上，我看一了webpack的文档，终于找到了原因，include是需要完全匹配的，所以我需要将include的value改成`/node_modules|iconfont/`，嗯，做事之前一定要先阅读好说明书！


### 如何提高页面加载速度
对于这个问题，项目依赖的东西是一定那么多的，如果是依靠文件什么的来提高加载速度，感觉不太现实，东西就那么多，只能这样那样拆来进行优化。

为了提高加载速度，项目也从http1换成了http2，看过这个概念，但是现在是想不起来了。

怎么说呢，提高加载速度有很多很多办法，但每种办法的收益不同，不要因为是做前端的而限制自己的视野。优化是一个持续的工作...

