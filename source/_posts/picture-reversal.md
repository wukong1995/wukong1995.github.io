---
date: 2016-07-16 12:23:11
title: css+js实现图片反转效果
tags: [css, javascript]
categories: [前端, javascript]
description: css+js实现图片反转效果
---

一个图片，点击图片图片反转180deg后，出现图片的简介
```html
<div class="container">
  <div class="photo-wrap photo-front">
    <div class="side side-front">
  	 <img src="img/banner.jpg">
    </div>
    <div class="side side-back">
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolorem aliquid laboriosam a ipsam ducimus ea nobis officiis dignissimos consequuntur asperiores fuga illum saepe rem eius ipsa vel, atque eos optio?</p>
    </div>
  </div>
</div>
```

```css
* {
	margin: 0;
	padding: 0;
	font-family: 'microsoft yahei';
}
.container {
	width: 220px;
	height: 280px;
	margin: 0 auto;
	margin-top: 50px;
}
.photo-wrap,.side {
	width: 100%;
	height: 100%;
}
.photo-wrap {
	transform-style: preserve-3d;
	transition: all .3s;
	position: relative;
}
.photo-wrap.photo-front {
	transform: rotateY(0deg);
}
.photo-wrap.photo-back {
	transform: rotateY(180deg);
}
.side {
	position: absolute;
	top: 0;
	left: 0;
}
.side img{
	width: 100%;
	height: 100%;
	backface-visibility:hidden;
}
.side-front {
	transform: rotateY(0deg);
}
.side-back {
	background-color: #fff;
	box-sizing: border-box;
	border: 5px solid #000;
	padding: 5px;
	transform: rotateY(180deg);
}
```

```js  
var photo_wrap = document.querySelector('.photo-wrap');
  photo_wrap.onclick = function() {
    var str = this.className;
    if(/photo-front/.test(str)) {
  	 this.className = str.replace(/photo-front/,'photo-back');
    } else if(/photo-back/.test(str)) {
  	  this.className = str.replace(/photo-back/,'photo-front');
    }
  }
}
```




