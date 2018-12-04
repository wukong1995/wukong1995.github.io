---
  title: js-picture-slide
  date: 2016-05-07 18:14:25
  articleTitle: 利用视觉差实现图片滑动
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---


今天看了别人写的图片滑动，看起来很酷，读源码时，似乎有些困难，就模仿着写了一个，实现的效果与原网页相同，不过自己的js代码，逻辑简单，有待改进。
ps：前两天写了旋转木马，那个兼容性不好，今天写这个网页的时候，也是按照这个思路，在谷歌浏览器上运行很好，火狐很多功能不能实现，由于wrap——panel使用了绝对定位，就将translate平移改为了left。改动后，各个浏览器运行的效果不错

```html
<div class="container">
    <h1>Parallax Slider</h1>
	<div class="wrap">
	    <div class="bg-img">
	    	<div id="bg_1" class="bg bg-1" style="left: 0px;"></div>
	    	<div id="bg_2" class="bg bg-2" style="left: 0px;"></div>
	    	<div id="bg_3" class="bg bg-3" style="left: 0px;"></div>
	    </div>
          <div id="wrap_panel" class="wrap-panel" style="left: 0px;">
			<div class="panel panel-1">
				<img id="img_1" src="images/1.jpg">
			</div>
			<div class="panel panel-2">
				<img src="images/2.jpg">
			</div>
			<div class="panel panel-3">
				<img src="images/3.jpg">
			</div>
			<div class="panel panel-4">
				<img src="images/4.jpg">
			</div>
			<div class="panel panel-5">
				<img src="images/5.jpg">
			</div>
			<div class="panel panel-6">
				<img src="images/6.jpg">
			</div>
          </div>
	    <div class="navigation-button">
	    	<span id="perv_btn" class="perv-button"></span>
	    	<span id="next_btn" class="next-button"></span>
	    </div>
		<div id="show_small" class="show-small">
			<ul>
				<li><img src="images/thumbs/1.jpg"></li>
				<li><img src="images/thumbs/2.jpg"></li>
				<li><img src="images/thumbs/3.jpg"></li>
				<li><img src="images/thumbs/4.jpg"></li>
				<li><img src="images/thumbs/5.jpg"></li>
				<li><img src="images/thumbs/6.jpg"></li>
			</ul>
		</div>
	</div>
</div>
```

```css
* { margin: 0; padding: 0; }
html, body, .container { width: 100%; height: 100%; font-family: 'Microsoft Yahei'; }
.container { background-color: #222; overflow-x: hidden; }
.container h1 { font-size: 50px; color: #ccc; text-align: center; font-weight: bolder; height: 120px; line-height: 120px; }
.wrap { position: relative; width: 600%; height: 400px; border-top: 10px solid #333; border-bottom: 10px solid #333; margin-top: 20px; }
.bg { position: absolute; width: 100%; height: 100%; left: 0; top: 0; -webkit-transition: all 1s;-moz-transition: all 1s;-ms-transition: all 1s;-o-transition: all 1s;transition: all 1s; }
.bg-1 { background: url(images/bg1.png); }
.bg-2 { background: url(images/bg2.png); }
.bg-3 { background: url(images/bg3.png); }
.wrap-panel { position: absolute; width: 100%; height: 100%; -webkit-transition: all 1s; -moz-transition: all 1s; -ms-transition: all 1s; -o-transition: all 1s; transition: all 1s; }
.panel { width: 16.66%; height: 100%; float: left; }
.panel img { display: block; margin: 0 auto; margin-top: 35px; border-radius: 10px; border: 10px solid rgba(143, 143, 143, 0.6); }
.navigation-button span:hover { opacity: 0.8 }
.perv-button, .next-button { position: absolute; width: 30px; height: 60px; background-color: #344133; border-radius: 10px; cursor: pointer; opacity: 0.4; }
.perv-button { background: #000 url(images/prev.png) center center no-repeat; }
.next-button { background: #000 url(images/next.png) center center no-repeat; }
.show-small { position: absolute; width: 680px; bottom: 20px; }
.show-small ul { list-style: none; }
.show-small ul li { float: left; margin: 0 10px; border: 5px solid #fff; opacity: 0.7; cursor: pointer;-webkit-transition: all .3s; -moz-transition: all .3s; -ms-transition: all .3s;-o-transition: all .3s; transition: all .3s; }
.show-small ul li:hover { margin-top: -15px; }
```

```js
window.onload = function() {
			// 得到元素
	var getDOM = function (id){
	    return typeof id==="string"?document.getElementById(id):id;
	}

	// 得到对象
	var img = getDOM('img_1');
	var prev = getDOM("perv_btn");
	var next = getDOM("next_btn");
	var wrap_panel = getDOM('wrap_panel');
	var bg_1 = getDOM("bg_1");
	var bg_2 = getDOM("bg_2");
	var bg_3 = getDOM("bg_3");
	var show_small = getDOM("show_small");
	var list = show_small.getElementsByTagName("li");
	var wwidth;

	 // 为元素绑定事件
	var addEvent = function(id,event,fn) {
	  var el = getDOM(id) || document;
	  if(el.addEventListener){
	    el.addEventListener(event,fn,false);
	  }else if(el.attachEvent){
	    el.attachEvent('on' + event,fn);
	  }
	}
	function init() {

		// 对按钮进行定位
		// 向前按钮的左边距离=图片的左距离+边框
		prev.style.left = img.offsetLeft + 10 + 'px';
		// 向前按钮的上边距离=图片的上距离+图片高度的一半-按钮高度的一半
		prev.style.top = img.offsetTop + img.clientHeight/2 - prev.clientHeight/2 + 'px';
		next.style.left = img.offsetLeft + img.clientWidth + 10 - next.clientWidth + 'px';
		next.style.top =prev.style.top;

		// 对小图片的容器进行定位
		wwidth = document.documentElement.clientWidth || document.body.clientWidth;
		show_small.style.left = (wwidth - show_small.clientWidth)/2 + 'px';
	}

	// 小图片的处理
	function small_img() {
		// 对图片进行旋转处理
		for (var i = 0;i< list.length; i++) {
			// 旋转方向
			var direction = Math.pow(-1,parseInt(Math.random()*10));
			list[i].style = "transform:rotate(" + (Math.random()*20*direction) + "deg)";
		}
		list[0].style.opacity = 1;
	}

	function only_one(el,num) {
		for (var i = 0; i < el.length; i++) {
			el[i].style.opacity = 0.7;
		}
		// console.log(num);
		el[num].style.opacity = 1;
	}

        // 浏览器缩放时
	window.onresize = function() {
		init();
	}

	// 执行函数
	init();
	small_img();
	addEvent(prev,'click',function() {
		// 改变wrap-panel的left
		var oldPos = parseInt(wrap_panel.style.left);

	    if(oldPos == 0) {

	       // 背景平移 图片容器平移
               bg_1.style.left=bg_2.style.left=bg_3.style.left=wrap_panel.style.left = -wwidth*(list.length-1) +'px';

               // 更改对应小图片透明度
               only_one(list,list.length-1);
	    } else {

	    	// 背景平移 图片容器平移
	       wrap_panel.style.left = (oldPos + wwidth) +'px';
	       bg_1.style.left= (oldPos + wwidth - parseInt(-(oldPos/wwidth + 1))*100) +'px';
	       bg_2.style.left= (oldPos + wwidth - parseInt(-(oldPos/wwidth + 1))*300) +'px';
	       bg_3.style.left= (oldPos + wwidth - parseInt(-(oldPos/wwidth + 1))*500) +'px';

	        // 更改对应小图片透明度
	        only_one(list,parseInt(-(oldPos/wwidth + 1)));
	    }
	});
	addEvent(next,'click',function() {
		// 改变wrap-panel的left
		var oldPos = parseInt(wrap_panel.style.left);

	    if(oldPos == -wwidth*(list.length-1)) {

	    	// 背景平移 图片容器平移
               bg_1.style.left=bg_2.style.left=bg_3.style.left=wrap_panel.style.left = '0px';

                // 更改对应小图片透明度
               only_one(list,0);
	    } else {
	    	// 背景平移 图片容器平移
	       wrap_panel.style.left = (oldPos - wwidth) +'px';
	       bg_1.style.left= (oldPos - wwidth + parseInt(-(oldPos/wwidth + 1))*100) +'px';
	       bg_2.style.left= (oldPos - wwidth + parseInt(-(oldPos/wwidth + 1))*300) +'px';
	       bg_3.style.left= (oldPos - wwidth + parseInt(-(oldPos/wwidth + 1))*500) +'px';

	        // 更改对应小图片透明度
	       only_one(list,parseInt(-(oldPos/wwidth - 1)));
	    }
	});
}
```


