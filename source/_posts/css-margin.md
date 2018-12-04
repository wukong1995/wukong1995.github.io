---
title: css中margin的巧妙使用
date: 2016-07-16 18:40:38
tags: ['css']
categories: ['css']
---
今天在慕课上看了张大神又一力作，整理了一下
margin的巧妙使用：
1、margin实现自适应的宽高比为2：1的矩形

```html
<div id="container">  
    <div class="box">  
       <div></div>  
    </div>  
</div>  
```
```
<style type="text/css">  
  #container {  
    width: 400px;  
    height: 250px;  
  }  
  .box {  
    background-color: olive;  
    overflow: hidden;  
  }  
  .box > div {  
    margin: 50%;  
  }  
</style>
```

![](http://upload-images.jianshu.io/upload_images/7018384-24ac7fb1fccbcec6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![](http://upload-images.jianshu.io/upload_images/7018384-70156a9605e570a9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![](http://upload-images.jianshu.io/upload_images/7018384-92ecf8c007214a71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

