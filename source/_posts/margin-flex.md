---
title: margin+flex=love
date: 2019-04-16 19:23:27
tags: [css]
categories: [css]
description: margin和flex使用产生的化学效应
---

[原因在这里](https://medium.freecodecamp.org/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af)
为什么要写这篇文章呢？之前在一个总结中说过这个问题，这次单拎出来的原因是：之前印象太浅了

### margin和flex
如果想要一个盒子居中显示，我可以加上margin: 0 auto;
如果想要一个盒子居右显示，我可以加上margin-left: auto;
那么我在flex中，对于flex-item使用margin会有什么效果呢？margin应用在flex-item上会覆盖它的主轴和纵轴的指定方式。

### 可以实现这样的布局
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1555413952/blog/flex_margin.jpg)
