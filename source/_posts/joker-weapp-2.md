---
title: 没写小程序之前，我还是快乐的空空（上篇）
date: 2018-08-24 17:55:31
tags: ['小程序']
categories: ['小程序']
---

如何在小程序里面保存图片？

#### plan
小程序可以将canvas保存成一张图片。现在主要的任务是将canvas中的内容画出来
web端有一个叫做html2canvas，对于小程序，暂时没有找到相似的工具库...只能自己来画了
目前，需要画出来的是：
1. 背景纹理
2. title
3. 图片


#### progress
##### 设置画布的大小
```javascript
ctx.rect(0, 0, width, height);
```
这里的遇到的问题，如何得到画布的高度...~~我先用html写出来结构，得到内容的高度~~，另一种方法是先去计算，emmm...这种方法还没想好思路


**8-27更新**：使用html的高度，和canvas画出来的排版不一样，不同的手机上的排版还不一样，最后导致的问题是在不同的手机上导出的图片要不是过短要不就是下面长出一截，最好还是一个一个元素的计算吧，确定每个元素的起点的y坐标
```javascript
heightInfo.hrTop = heightInfo.xxxxxTop + titleInfo.height + 20;
heightInfo.xxxxTop = heightInfo.hrTop + 24;
heightInfo.xxxTop = heightInfo.xxxxTop + 14 + 20;
heightInfo.xxxxTop = heightInfo.xxxTop + xxxInfo.height + 36;
heightInfo.xxxxTop = heightInfo.xxxxTop + 90 + 7;
this.height = heightInfo.xxxxTop + 14 + 30;
```
在这个过程中可以直接将多行字符串分割....还有一个将数字定义成常量

------

##### 画背景纹理，每隔10画一条横线，竖线
这个是重复画线的一个过程
```javascript
drawLine: function(fromX, fromY, toX, toY, lineWidth = 1) {
  this.ctx.beginPath();
  this.ctx.setLineWidth(lineWidth)
  this.ctx.moveTo(fromX, fromY);
  this.ctx.lineTo(toX, toY);
  this.ctx.stroke();
},

// ....
const step = 10;
const countX = width / step;
const countY = height / step;
for (let i = 0; i < countX; i++) {
  drawLine(i * step, 0, i * step, height);
}

for (let i = 0; i < countY; i++) {
  drawLine(0, i * step, width, i * step);
}
```
##### title
涉及到文字换行，canvas不能主动对超出的文字做换行处理，处理的方法是拿到字符片段长度与最大宽度依次比较

```javascript
const arrText = text.split('');
const line = '';
for (let n = 0; n < arrText.length; n++) {
  const testLine = line + arrText[n];
  const metrics = this.ctx.measureText(testLine);
  const testWidth = metrics.width;
  if (testWidth > maxWidth && n > 0) { // 超出一行，打印
    ctx.fillText(line, x, y);
    line = arrText[n];
    y += lineHeight;
  } else {
    line = testLine;
  }
}
ctx.fillText(line, x, y);
```
##### 图片
这个涉及到居中显示
```javascript
// 计算出图片的起始点即可
// use drawImage
```

#### 过程中遇到的问题
canvas的层级过高，会遮盖view元素，解决办法是使用：croll-view
另一个问题是：croll-view设置border或者box-shadow不起作用，次而求之，再用一个croll-view假装边框吧....



