---
title: webpack的hash、chunkhash、contenthash
date: 2020-07-16 20:57:26
tags: [webpack]
categories: [前端, webpack]
description: webpack的三个hash你了解吗？
---

对于webpack的hash，常用于cdn缓存。我理解的是文件不变的情况下，最后打包出来的hash串也不会变。最近被问到了这是三个hash的区别，就查了一下，发现还很有讲究。

先看一下三个hash的解释：
- [hash] is a "unique hash generated for every build"
- [chunkhash] is "based on each chunks' content"
- [contenthash] is "generated for extracted content"

### 这里有两次代码变更
原代码：
```js
// file1.js
console.log('file1')

// file2.js
console.log('file2')

// file3.js
console.log('file3')

// index.js
require('./file2')
console.log('index')

// detail.js
require('./file1')
console.log('detail')

// webpack.config.js
const path = require('path')
const webpack = require('webpack')

module.exports = {
  // mode: 'development',
  // mode: 'production',
  entry: {
    index: './src/index.js',
    detail: './src/detail.js',
  },
  output: {
    filename: '[name].[hash].js',
    path: path.resolve(__dirname, 'dist')
  },
}
```

第一次变更：
```js
// file2.js
console.log('file22')
```

第二次变更：
```js
// index.js
require('./file2')
require('./file3')
console.log('index')
```

下面我会以我理解的顺序比较一下三个hash
### hash
每次构建的生成唯一的一个hash，且所有的文件hash串是一样的。
源代码构建：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594809343/blog/hash.png)

第一次变更：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594809340/blog/hash-change.png)

是不是看到非预期的地方了？我只改了file2.js，index.js的hash串变了，但是为什么detail.js为什么也变了？这还怎么有缓存的作用！不行，升级！

### chunkhash
每一个文件最后的hash根据它引入的chunk决定

源代码构建：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594809339/blog/chunkhash.png)

第一次变更：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594809353/blog/chunkhash-change.png)
这次文件的hash变化符合预期，index的变了，detail的没变

第二次变更：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594809340/blog/chunkhash-change2-add-chunk.png)
你会发现这次变更也是基于index的变更，但是实际上detail的文件内容没有变，那为什么它的hash也跟着变了？

>原因是 module identifier，因为 index 新引入的模块改变了以后所有模块的 id 值，所以 detail 文件中引入的模块 id 值发生了改变，于是 detail 的 chunkhash 也随着发生改变。

不用怕，webpack已经提供方案了，解决方案是将默认的数字 id 命名规则换成路径的方式。webpack 4 中当 mode 为 development 会默认启动，但是production环境还是默认的id方式，webpack也提供了相应的plugin来解决这个问题
```js
plugins: [
    new webpack.HashedModuleIdsPlugin(),
],
```
加上这个plugin后，再走一遍上述代码的变更，你会发现第一次、第二次的变更后，detail的hash串仍然没有变化，符合预期。

在webpack中，有css的情况下，每个entry file会打包出来一个js文件和css文件，在使用chunkhash的情况下，js和css的文件的hash会是一样的，这个时候暴露出来的一个问题：你修一个react的bug，但是并没有改样式，最后更新后，js和css的文件的hash都变了。这个还是不太好，css文件的hash串不变最好，再继续升级！

### contenthash
contenthash是根据抽取到的内容来生成hash。

生产环境是不是会使用一个`MiniCssExtractPlugin`来进行css的压缩，这个时候我们在这个plugin里面指定hash为`contenthash`，你会发现修改js文件后，js文件的hash串变了，css的hash串没变！完美。

```js
new MiniCssExtractPlugin({
  // Options similar to the same options in webpackOptions.output
  // both options are optional
  filename: '[name].[contenthash:8].css',
  chunkFilename: '[name].[contenthash:8].chunk.css'
})
```

将webpack中的全部hash都设置成contenthash的情况下，仅仅只修改css文件，不改js文件的情况下，css文件的hash串会变，js文件的不会变，这样能达到最小更新。

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594957667/blog/contenthash2.png)

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594957667/blog/contenthash2-changecss.png)

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1594957667/blog/contenthash2-changejs.png)

### 其他
我查了两个流行的脚手架：create-react-app和umi，发现它们的entry file的配置都是contenthash
```js
output: {
    filename: '[name].[contenthash].js',
    chunkFilename: 'cfn_[name].[contenthash].js',
  },
```
umi使用了`HashedModuleIdsPlugin`来进行稳定的hash构建，但是cra没有，我看有人已经提issue了：[https://github.com/facebook/create-react-app/issues/6909](https://github.com/facebook/create-react-app/issues/6909)，作者说`optimization.moduleIds: "hashed"`这个也能满足需求，查了[webpack5中optimization.moduleIds](https://webpack.js.org/configuration/optimization/#optimizationmoduleids)是可以的

所以目前最佳实践是`contenthash`+`HashedModuleIdsPlugin/optimization.moduleIds: "hashed"`

参考资料：
[https://stackoverflow.com/questions/35176489/what-is-the-purpose-of-webpack-hash-and-chunkhash](https://stackoverflow.com/questions/35176489/what-is-the-purpose-of-webpack-hash-and-chunkhash)
[https://imweb.io/topic/5b6f224a3cb5a02f33c013ba](https://imweb.io/topic/5b6f224a3cb5a02f33c013ba)
