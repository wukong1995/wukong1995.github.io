---
title: 一次乌龙的排查-webapck重复打印log
date: 2021-01-10 13:31:23
tags: [webpack]
categories: [webpack]
description: 你的问题如果在issue和stackoverflow中找不到答案，那一定是你错了
---

之前写了一个webpack的plugin，用来上传静态资源到oss上，它经过三个项目的考验，还是很实用的。这次在另一个项目中，使用了它，我发现jk的log中，上传的log竟然出现了三次！！！刚开始我怀疑是jk的问题，但是我在本地进行了一次打包，凉凉，发现本地上传的log也是发生了三次，于是我就开始找到底是哪的问题

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1610257162/blog/webpack-print-log-repeat.jpg)

##### 难道是webpack用的hook有问题
上传的plugin用的是afterEmit这个hook，它的解释是：
>Called after emitting assets to output directory.

项目也用了别的plugin，例如copyPlugin、htmlPlugin，这两个也都会释放文件到ouputPath中，难道它们会多次触发afterEmit，于是拿着“webpack afterEmit call multi times”去搜，但是没有找到相关的问题，于是我开始模拟这种webpack的配置

```js
// webpack.config.js
// ...
plugins: [
  new MyPlugin(),
  new CopyWebpackPlugin( [
      path.resolve(__dirname, "source")
    ],
  )
],

// my_plugin.js
class MyPlugin {
  constructor() {

  }

  apply(compiler) {
    this.doWith(compiler)
  }

  doWith(compiler) {
    compiler.hooks.afterEmit.tapPromise('MyPlugin', () => {
      console.log('afterEmit run run!!!')
      return Promise.resolve()
    })
  }
}

module.exports = MyPlugin
```
测试下来发现afterEmit只打印了一次。
我开始想是不是想错了，顺着两个另外的plugin的代码，像htmlPlugin它用的是Compilation Hooks中的processAssets来生成html，确定了我的猜想是错误的，afetrEmit只会执行一次，除非你recomplie了。

##### 难道是umi的问题
现在的项目是使用umi2生成中，其中的配置完全是黑盒，难道它里面同时开了两个compile，抱着这样的猜想，去找了umi中的webpack配置，发现和平时没有什么不同，我看了package.json中的依赖，有一个webpackbar，它是来显示webpack打包的进度，我尝试把它禁用，然后再执行一次buid，发现log上只打印了一次！！！我尝试用“webpack print log duplicate”搜索没有收获，找webpackbar的issue也没什么收获。

##### 最后的最后
看了一下webpack的hooks，反正都是上传文件，done之后传也行，为了安心，于是增加了一个配置，使用done这个hook来执行上传动作。

又一次验证了一个道理：你的问题如果在issue和stackoverflow中找不到答案，那一定是你错了。


