---
title: week5
articleTitle: 本周总结
date: 2017-08-26 17:27:09
tags: ['总结']
categories: ['随笔']
description: 代码中的bug
---

上周没写总结...

### if条件什么时候为false
因为react共用组件，但是传过来的值的结构不同，于是使用||来进行判断选择，于是写了以下代码放心的提上去了
```
const count = node.count || node.usage_count || node.total_count
```
发现报total_count是undefined，这个我仔细看了代码没找出来哪错了，于是把生产环境的数据库拿下来，开始调试...最后发现因为`usage_count`的值为零的时候，会继续往后执行，因为`node.usage_count`值为0，js会认为是false...
我的原意是为undefined的时候，会继续向后执行。使用js很随便，但是忘记了随便的副作用。于是我只能用if...else...去判断undefined了
👉当if语句中的变量为false，0，NaN，空字符串，null，undefined时，判断结果为假;

### 图片是使用背景图还是img标签
一般来说，我的习惯是图片一般使用img标签插入页面。
但是，假如一个网站有中文英文两个版本，通过类名的切换而不是跳转页面可以实现中英文的切换，这个时候，就不要img标签，而是背景图的形式插入图片，这样做的好处是：图片是在css设置的，我可以为元素设置不同的类名进而切换图片

### BEM不应该嵌套太深
BEM命名的方式，一般是一个block里面包含element,所以我是一个block一层来的。大哥告诉我这样是不对的，划分block没有错，但是在命名不冲突的情况下，block中的element的类名没有必要一定按照block的类名开头。

### 一定要选好元素
之前做tab切换的时候，咋改都没达到想要的效果。看了大哥的代码，才发现自己选错元素了。应该选择section的父元素而不是每个section...写代码之前没有经过严密的思考，遇到错误时，思想受到了限制，导致没想到正确的方向去...

### calc
css3中的calc这个计算属性超级好用，但是有一点需要注意
```css
height: calc(100%-75px);
```
以上代码不起作用，让人摸不着头脑，经过查询之后，需要注意的是`-`号两边要有空格
```css
height: calc(100% - 75px);
```

### display:flex;兼容性
网站要兼容到IE9，而flex是从ie10兼容的。找hack但是没有只对IE9起作用的hack...唉，使用`\9`的hack，它也在IE10下起作用
对于IE9的兼容，我一般使用`display: table;`和`display: table-cell;`。此时你在设置子元素的margin是不起作用的，此时你想要的效果这两个css属性可以达到你的需求`border-collapse: separate;`、`border-spacing: 5rem`。
