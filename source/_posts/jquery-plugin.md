---
title: 在webpack环境下，安装jquery插件不能使用
date: 2018-03-13 19:11:53
tags: [jquery, webpack]
description: 在webpack环境下，安装jquery插件不能使用，假如安装了jquery-form插件，使用ajaxform方法，提示jquery对象没有这个方法
categories: [前端, jquery]
---

### 使用插件
现在是使用webpack打包，yarn安装需要的插件很便捷。今天遇到了一个奇怪的问题，我使用yarn安装了一个jquery插件，使用时，jquery报错...

### 排错
1. 安装、引用是否正确
首先先确定一下：包是否正确安装；使用时是否正确引用；ok，这两个都是正确的

2. webpack是否配置正确
在webpack中，一般都会将jQuery设置为全局变量，在webpack中设置如下:
```javascript
new webpack.ProvidePlugin({
    $: "jquery",
    jQuery: "jquery"
})
```
3. webpack版本是否合适
开发中使用的是`rails-webpack`，其中的配置需要更改（之前对照文档修改配置，报错；今天才看到原来是版本问题😂）

4. 最最重要的一点：查看你安装的插件的包里面有没有`node_modules`这个文件夹
在排查错误时，我将包里面的代码拷贝到开发目录，我发现能用。。。直接引用包就不可以。使用插件，最终目的是在`$`这个对象上挂载方法。报错就是说明`$`对象上没有这个方法，问题来了，包里面的`$`是哪来的？全局对象还是`node_modules`文件夹中的`jquery`???
引用多个插件，插件依赖的`jquery`版本可能不是一样的，webpack打包的时候，首先去找安装包的插件`node_modules`里面的`jquery`，局部变量覆盖全局变量。所以此时挂载方法的`$`对象是`node_modules`包中的`jquery`，而不是全局的`$`对象

5. 为什么安装的jquery版本会不同
这个时候你也许会疑问🤔️为什么会安装这么多版本？首先你要去检查依赖的`jquery`版本，这个时候你可以去查看`yarn.lock`文件，里面有具体的依赖关系。这个时候，假如按照算法，你发现几个插件依赖的`jquery`的版本应该是一致的。但为什么`yarn`计算出来的不一样呢🤔️？因为`yarn`在`add`包时，计算`lock`的时候，会在原来的基础上进行计算，恰好在这个时间隔中，`jquery`升级了...这就导致`yarn`计算出来的依赖版本可能有所不同
此时，你可以使用`yarn upgrade`进行更新`lock`文件

6. 如果以上步骤还不能解决问题...我也不知道根本问题是什么








