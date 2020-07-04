---
title: webpack中优先打包node_modules中的style
date: 2020-07-04 06:32:03
tags: [webpack]
categories: [前端, webpack]
description: 一把辛酸泪==，在webpack cache group中先打包node_modules中的样式
---

## 第一次的解决
在网上看到的`node_modules`的打包都是放在一个chunk里面，但是对于大项目，如果继续这样，vendor文件简直大到暴，于是对`node_modules`进行了拆分，拆分的js文件大致符合预期，但是样式文件的顺序错乱了（之前用的css in js没暴露出问题，改成less后发现了），`node_modules`中样式文件穿插在了自己写的样式中间，导致好几个页面上样式出了问题。这个时候我又加了一个规则，想优先打包`node_modules`中的样式文件。

我的主要目的是将`node_modules`里面的样式文件先打包，但是`test`使用function，怎么也达不到目的
```js
// 先把css结尾的文件打包，主要是node_modules中的文件
styles: {
    name: 'styles',
    test: (module) => {
      const name = module.resource
      return /node_modules/.test(name) && /\.css$/.test(name)
    },
    chunks: 'all',
    enforce: true,
    priority: 20
}
```

为了解决这个问题，因为`laiye-antd`的按需加载使用的是css文件，不是less文件，于是就写了下面的正则，目前能解决样式文件顺序错乱的问题。

```js
// 先把css结尾的文件打包，主要是node_modules中的文件
styles: {
    name: 'styles',
    test: /\.css$/,
    chunks: 'all',
    enforce: true,
    priority: 20
}
```

## 二次爆发
新Q的计划是要将`laiye-antd`升级到基于antd4的1.x版本，但是样式上有一个很大的改动就是`@border-radius-base`从4px变成了2px，为了保证样式的兼容，我必须保证，即使升级了版本，组件的圆角仍然保持之前的4px。最容易想到的办法的是项目的配置不变，在组件库中改，但是该起来文件改的比较多，还有一个很大的原因，假如设计同学说我要把圆角从4px变成2px，这个时候我还要去发一次版本？这个很明显是不符合正常逻辑的。所以这个时候，我引用`laiye-antd`的时候，必须将css变成less，可满足随时css变量的修改。这个时候，我还是重回最初的问题：我要打包`node_modules`中的样式文件，我不管它是css结尾还是less结尾，最重点的是`node_modules`中的，于是开始了test之旅。

```js
// 先把css结尾的文件打包，主要是node_modules中的文件
styles: {
    name: 'styles',
    test: /\.css$/, // 这个是可以的
    test: (module) => { // 这个是不可以的
        return /\.css$/.test(module.resource)
    },
    chunks: 'all',
    enforce: true,
    priority: 20
}
```

我找了很多test为function的配置，基本上`rep.test`中是`module.resource`或者`module.context`，怎么改都不行，一直在圈里徘徊。

## 怎么解决
### 方案一：正则
我想要不还是正则吧，使用正则里面的与来解决，`node_modules`和`(c|le)css`结尾

```js
// 先把css结尾的文件打包，主要是node_modules中的文件
styles: {
  name: 'styles',
  test: /(?=node_modules).*(?=.(c|le)ss$)/,
  chunks: 'all',
  enforce: true,
  priority: 20
}
```
打包出来一看，可以可以，满足了我的需求。但是很明显这个正则的效率不太高。还是不如两个test并起来

### 方案二：function
我还是决定采用函数吧，我只能去看webpack的源码到底是怎么操作，到底问题出在哪里

```js
// SplitChunksPlugin.js
// 268行
else if (SplitChunksPlugin.checkTest(option.test, module)) {

}

// checkTest是何方神圣
static checkTest(test, module) {
  if (test === undefined) return true;
  if (typeof test === "function") {
      if (test.length !== 1) {
          return test(module, module.getChunks());
      }
      return test(module);
  }
  if (typeof test === "boolean") return test;
  if (typeof test === "string") {
      if (
          module.nameForCondition &&
          module.nameForCondition().startsWith(test)
      ) {
          return true;
      }
      for (const chunk of module.chunksIterable) {
          if (chunk.name && chunk.name.startsWith(test)) {
              return true;
          }
      }
      return false;
  }
  if (test instanceof RegExp) {
      if (module.nameForCondition && test.test(module.nameForCondition())) {
          return true;
      }
      for (const chunk of module.chunksIterable) {
          if (chunk.name && test.test(chunk.name)) {
              return true;
          }
      }
      return false;
  }
  return false;
}

// nameForCondition又是什么呢？NormalModule.js文件中
nameForCondition() {
  const resource = this.matchResource || this.resource;
  const idx = resource.indexOf("?");
  if (idx >= 0) return resource.substr(0, idx);
  return resource;
}
// 暂时先看到这个里
```

看到源码后，我尝试用`nameForCondition()`来代替`resource`

```js
styles: {
  name: 'styles',
  test: (module) => {
    const name = module.nameForCondition && module.nameForCondition()
    if (name && /node_modules/.test(name) && /.(c|le)ss$/.test(name)) {
      return true
    }

    return false
  },
  chunks: 'all',
  enforce: true,
  priority: 20
}
```

可以了，但是matchResource、resource到底有什么区别呢？我用console.log打印了一下

![webpack-cachegroup](https://res.cloudinary.com/dwudaridr/image/upload/v1593818216/blog/webpack-cachegroup.png)

可以看到有时候resource会是undefined的情况，内部的编译我也不清楚==。


再说一下module.chunksIterable，这个数组真的非常大，但是没关系，webpack5已经去掉了，在代码里面看不见这个循环了，期待5的正式版本😄

### 项目修改
```js
['import', { libraryName: 'laiye-antd', style: true }] // css => true

// less-loader里面加上
options: {
  sourceMap: true,
  javascriptEnabled: true,
  modifyVars: {
    'laiye-primary-color': 'red'
  }
}
```
目前已经解决，撒花🎉

## 总结
最后的最后，问题发生的原因还是因为我对webpack内部编译的原理不清楚，导致根据输入不能预估到输出，这是个很危险的事情。总之，继续学吧...



















