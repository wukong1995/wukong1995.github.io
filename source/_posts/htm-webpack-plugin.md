---
title: 使用htm-webpack-plugin构建最终的html
date: 2021-08-08 09:38:14
tags: [前端]
categories: [前端]
description: 一系列骚操作
---

#### 确定使用`html-webpack-plugin`
发现项目中有多个entry html，重复性极高，想去统一处理一下，只留两个entry： 一个pc端，一个mobile端。又看了一下项目的代码发现用的`html-res-webpack-plugin`，这个包去npm上查了一下发现更新已经是两年前了，所以这个是要干掉的。之前一直都用`html-webpack-plugin`，查了一下它的功能，发现还是可以满足我的需求的，于是我决定使用它来重构。另外项目中还有一些`pug`、 `ejs`的loader，用上的话感觉有点重，所以还是只留一个plugin吧


#### 要满足的功能：
首先整理一下构造一个最终的html，需要哪些点？
1. 支持html partial：这样html回看起来比较清爽
2. 支持css js文件inline插入：项目中有一些inline需要插入，这个也似于partial
3. 支持图片链接的转换：html中会引用一些文件，希望文件可以通过webpack的打包
4. 支持变量的替换： html中有动态的变量
5. 支持打包后文件的注入

#### 如何解决
###### partial引入
4和5是plugin支持的功能，其余的html-loader能满足，于是我加入了这样的配置
```js
test: /\.html$/,
use: [
  { loader: 'html-loader' },
]
```

但是实际用起来发现我的plugin不起作用啊，为什么会这样呢？在issuse中找到一个答案：html文件经过webpack的打包会变成`module.export`的形式，这样根本就走不到plugin，解决的办法可以将html变成ejs。ejs写起来太麻烦了，不想变成ejs，于是继续搜啊搜，终于找到了另一个办法`<%= require('html!./partial.html').export %>`，哈哈，想回复中说的，`<%= %>`是js代码，你可以任意写你想要的，那么2如法炮制，也采用这样的做法

```html
<style>
  <%= require('css-loader!./index.css').default %>
</style>
<script>
  <%= require('babel-loader!./test.js') %>
</script>
```

如果js不想进行打包也可以使用`raw-loader`，它直接将文件内容处理成一个string变量

###### 图片的引入
3的话，可以将引入图片的html做成一个partial，使用html来处理
```html
<%= require('html-loader!./partial.html').default %>
```
或者也可以这样
```html
<img src="<%= require('./logo.png') %>">
```
感觉第一种方法会简单一些

另外迁移到项目中遇到的问题：js已经被loader打包了，单独指定不会有作用, 需要使用exclude排除掉。在解决的过程中，有一句话也很有意义：

>最好不使用copy-webpack-plugin，会跳过webpack编译，失去webpack的一些功能


参考链接：
* [is-there-a-way-to-include-partial-using-html-webpack-plugin](https://stackoverflow.com/questions/42193689/is-there-a-way-to-include-partial-using-html-webpack-plugin)
* [https://github.com/jantimon/html-webpack-plugin/issues/1400](https://github.com/jantimon/html-webpack-plugin/issues/1400)
* [html-loader-overwrite-htmlwebpackplugin-expression](https://stackoverflow.com/questions/54488237/html-loader-overwrite-htmlwebpackplugin-expression)
* [how-does-html-webpack-plugin-work-with-html-loader](https://stackoverflow.com/questions/47996190/how-does-html-webpack-plugin-work-with-html-loader)
* [html-loader-overwrite-htmlwebpackplugin-expression](https://stackoverflow.com/questions/54488237/html-loader-overwrite-htmlwebpackplugin-expression)

