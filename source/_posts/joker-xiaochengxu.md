---
title: 我经历了小程序的哪些坑
date: 2018-08-17 23:28:20
tags: ['小程序']
categories: ['小程序']
---

在这个可爱的节日里，让我来说说，写小程序的坑～

对小程序的期待就像对vue的期待一样，看着文档就能写...可是，我踩了一个又一个的坑...

#### textarea的padding问题
为`textarea`设定了上下12rpx的内边距，在安卓机上十分完美，但是从测试那边反馈的结果是：框太高了...于是就去看看网上的讨论，在这吐槽一下，度娘真的是用中文都检索不出来结果...查了结果之后，发现textarea的内边距在安卓和iOS上的差别很大...我有以下方案：
1. 不使用`textarea`的内边距，外面套一层view，为view添加内边距。实践的结果是：不行。看到有答案说：`textarea`使用的原生组件，任何`padding`、`line-height`都对此不起作用
2. 为`view`设置`box-sizing: border-box`，这么一设置，`textarea`的`auto-height`都不起作用了，扎心...
3. 没办法了，我只能去区别手机系统了：
```js
wx.getSystemInfo({
  success: (res) => {
    if (res.model.match('iPhone X') !== null) {
      this.globalData.isIphoneX = true;
    }
    if (res.platform === 'ios') {
      this.globalData.isAndroid = false;
    }
  }
})
```
view层通过判断`getApp().globalData.isAndroid`的值，如果为`true`，则加padding，否则，不加 padding。

#### iphone的橡皮筋效果
如果你的页面有header、body、footer，header是白色，body是灰色，footer是白色，page本身是灰色，那么你在iphone上上拉页面，会看到灰中一块白，如何去调整，当然是遮盖再遮盖...这个效果，体验特别差...

#### iphone X的安全边距
找到的资料是安全边距是`68rpx`，这个时候，如果你有一个固定到下方的`footer`，和出现上面的问题，那么一定得好好想想如何去遮遮遮...

#### input和textarea的选择
`textarea`有一个`auto-height`的属性，可以自动根据内容去改变高度；但是`input`有手机键盘会有完成而不是回车的键，同时也会有一个点击完成键盘不收起的选项；总之你可以根据这两个特性去选择你需要的组件，加入需要`input`的属性，又需要`textarea`的属性，`input`是最好的选择...以我的经验来说

#### 页面即有上拉加载又有下拉刷新
一定要选用`Page`的特性，而不是`scroll-view`

#### 打开键盘是否推起页面
先考虑不推起页面，再考虑推起页面...因为推起页面显得特别不自然....

#### 得到globalData中的值
如果你想以下面的格式来得到`globalData`的值，可能会出乎你的意料
```js
Page({
  data: {
    isIphoneX: app.globalData.isIphoneX, // 同步得到，可以得到正确的值
    isLogin: app.globalData.isLogin,     // 异步得到，得不到正确的值
  },
})
```
没办法，我只能在`page`的`onload`中再重新赋值一次....

8-17，先写到这...




