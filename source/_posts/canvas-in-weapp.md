---
title: 在小程序中画图的第二次体验
date: 2018-10-29 20:00:34
tags: ['小程序']
categories: ['小程序']
---

今天的主要任务是画一个分享的海报，基于之前的一次经验，这次遇到的坑比较少，也更有耐心了🙂️

1. canvas中绘制网络图片
canvas中的drawImage中的第一个参数是图片的地址，但是不能是网络图片，解决的办法是：先下载下来，再去绘制
```javascript
wx.downloadFile({
  url: this.data.article.cover_image_url,
  success: (res) => {
    if (res.statusCode === 200) {
      // cover
      this.ctx.drawImage(res.tempFilePath, 0, 0, width, height);
    }
  }
});
```
给图片来再来个遮罩
```js
this.ctx.setFillStyle('rgba(40, 40, 40, 0.3)');
this.ctx.fillRect(0, 0, width, height);
```

2. 基于canvas在小程序中的是原生组件实现的，层级高于普通元素，怎么将它隐藏？
```css
opcity: 0;
visibility: hidden;
```
测试了一下是不可以的....
```css
 position: absolute;
left: 10000rpx; // 给它一个巨大的偏移
```
之前是做了一个切换，需要绘制canvas的时候就显示出来，但是这样会有一个闪烁的效果，所以想着有没有一个能让canvas看不见又能导出图片的方法，方法一在虚拟机上是可行的，但是在真机上canvas是能显示出来的...plan A pass，幸运的是plan B可行....赞

3. canvas中的最重要是获得canvas的实际高度
之前试过直接拿着dom的高度去设置canvas的高度，但是每个手机上的表现是不一样的，有的可能过长，有的可能过短，总之很难预测...canva中排版字体的时候，由于不能自动换行，只能去手动的计算，可能导致的后果是符号显示在行首，正常使用dom的话，通过设置可以避免符号在行首，这样以来，两者对于文字的排版是不一样的，从而高度也是不一样的

4. 补充：在setFillStyle后使用drawImage绘制图片
我的图片本来是白的，但是看起来却是灰的，原因竟是我在前面的setFillStyle的操作！！！





