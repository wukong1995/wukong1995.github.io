---
date: 2016-06-01 16:34:09
title: offsetTop和offset().top
tags: [javascript]
categories: [前端, javascript]
description:
---


前段时间写了一个视觉差滚动的demo，是用js实现的。
第二次看这个例子，我想封装一个jquery插件。
首先demo中有两个button需要在jq中对它们进行定位，然而问题来了

我先得到第一张图片的位置，对图片中的button的top进行定位时，想法是将css样式中的top设置成`this.img.offset().top + this.img.height() / 2 - this.prev.height() / 2 + 'px'`，然而却出现了上图的效果，`button`跑到了下方。将`this.img.offset().top`改为`this.img.position().top`依然无法出现我想要的效果 ，什么原因呢？
在用js实现时，也是采用的这种思想，用的是`img.offsetTop + img.clientHeight/2 - prev.clientHeight/2 + 'px'`，这个是正常显示的。
于是在例子中我打印出来`this.img.offset().top `和 `img.offsetTop`和`this.img.position().top` 的值，分别是185，35，0，这个 是什么原因呢？
经过测试，offset()得到的结果永远是相对于文档的偏移值，它会忽略元素所有的父元素；
查了一下资料，其中position()属性是获取它最近的具有相对位置（position:relative）的父级元素的距离，如果找不到这样的元素，则返回相对于浏览器的距离。可是，这样得出的0值不符合我的预期，在html结构中，离img最近的relative元素是.wrap元素（请参见另一篇文章http://blog.csdn.net/u013742084/article/details/51339213），想象中应该是35才对，这一次是怎么也想不通了。。。。待后续补充。。。
offsetTop获取它最近的具有相对位置（position:relative）的父级元素的距离，如果找不到这样的元素，则返回相对于浏览器的距离。这是一个相当纠结的属性；
我做了一个测试。

```html
<h1 style="text-align: center;background-color: #222;">这是一个大大的标题</h1>
<div id="container">
    <div class="panel" id="panel1"></div>
    <div class="panel" id="panel2"></div>
    <div class="panel" id="panel3"></div>
</div>
```

```css
* {
    margin: 0;
    padding: 0;
    font-family: 'Microsoft Yahei'
}
a {
    color: #000;
    text-decoration: none;
}
.panel {
    margin:20px auto 20px auto;
    width:80%;
    height: 500px;
    transition: all .3s;
}
#panel1 {background-color: red;}
#panel2 {background-color: green;}
#panel3 {background-color: blue;}
```

打印id为`panel1`的`offsetTop`、`offset().top`、`position().top`的值为62 62  42
我为id为container的div添加样式：  `style="position: relative;"`结果是 0 62 -20，百思不得其解
我有尝试了一个新的思路，在原有的上面的html结构上，添加两个操作：为id为container的div添加样式：  `style="position: relative;"`，在container中，panel1之上添加了一个h1的标签，panel的三个值变为了 62 104 42，
请大大的关注offsetTop这一属性，原先没有h1标签时，它的值是0，可是添加了h1后他的值变为了62，请注意h1的高度只有42，那个20 是从哪来的，唯一的解释是panel1的margin-top是20px，这么一想，脑袋又乱了，margin-top怎么又起作用了？想不通，，，
先写到这，等后续补充。




