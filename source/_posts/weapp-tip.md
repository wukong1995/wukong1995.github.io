---
date: 2018-09-01 09:48:48
title: 小程序总结
tags: [小程序]
categories: [小程序]
---


1. 如果你使用了`canvas` 、`map`类的组件，因为小程序是使用原生控件，无论你是怎样调整`z-index`，都不可能覆盖到这类组件上。网传的`z-index`设置为`1000`以上，经过实践是不起作用的。请使用`cover-view`。但是`cover-view`无法增加shadow和border。border可以通过使用`cover-view`的背景色来代替。shadow可以使用`cover-image`来代替。

2. 正确的选择`scroll-view`和`page`。两者都能实现下拉刷新，上拉加载。通过业务，选择最适合的方案。

3. 自定义导航栏时，通过`getSystemInfo`得到`statusBarHeight`在页面上使用px定义，而不是rpx来定义。

4. 自定义导航栏时，如果有监控page的滚动的需求，请选择`scroll-view`来实现。在使用`onPageScroll`时，在iphonex上会导致上方自定义的导航栏抖动。测试的时候，会看到页面滚动时，iphonex上方有1px的空隙....

7. 小程序在`iphonex`的下方的安全边距为`68rpx`

8. ios上会有自带的橡皮筋效果，记得保证页面下部的颜色与page的背景色相同

9. 如果想实现保存图片的功能，需要通过canvas去画出图片，canvas的高度需要按照实际的内容计算出来。

10. 待续...
