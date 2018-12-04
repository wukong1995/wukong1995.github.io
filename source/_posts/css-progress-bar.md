---
  title: CSS3实现炫酷进度条
  date: 2016-06-10 13:32:28
  tags: ['css']
  categories: ['前端', 'css']
  description:
---

看了一个进度条很漂亮，所以自己试试看

```html
<div class="load-container">
	<span class="run"></span>
	<div class="meter">0</div>
</div>
```


```css
* {
	margin: 0;
	padding: 0;
	font-family: 'microsoft yahei';
}
html,body {
	width: 100%;
	height: 100%;
	background-color: #222;
}
.load-container {
	width: 600px;
	height: 6px;
	margin: 0 auto;
	margin-top: 200px;
	background-image: -webkit-linear-gradient(left,#5bd8ff, #ff0000);
	border-radius: 5px;
	position: relative;
}
.run {
	position: absolute;
		width: 0px;
		height: 6px;
		right: 0px;
		background: #000;
		border-radius: 5px;
		animation: runnAnimation 10s linear;
}
@keyframes runnAnimation {
	0% {
		width: 600px;
	}
	100% {
		width: 0px;
	}
}
.run:after {
	content: "";
	display: block;
	width: 16px;
	height: 16px;
	border-radius: 50%;
	background-color: #f00;
	margin-left: -4px;
	margin-top: -4px;
	animation: destination 10s linear;
}
@keyframes destination {
	0% {
		background-color: #5bd8ff;
	}
	100% {
		background-color: red;
	}
}
.meter {
	float: right;
	margin-top: 10px;
		font-size: 40px;
		color: red;
		animation: fontColor 10s linear;
}
@keyframes fontColor {
	0% {
		color: #5bd8ff;
	}
	100% {
		color: red;
	}
}
.meter:after {
	content: "%"
}
```

```js
window.onload = function() {
	var meter = document.querySelector('.meter');
	run(meter,100);

	function run(el,time) {
		time = time ? time : 100;
		el = el ? el : document;
		var i = 0;
		var timer = setInterval(function() {
			if(i<100) {
				i++;
				el.innerHTML = i;
			} else {
				clearInterval(timer);
			}
		},time)
	}
}


如果想要调整进度时间，需要修改css样式中animation的时间和js中的时间


PS：css未做前缀处理，所有测试都在最新谷歌浏览器下进行


