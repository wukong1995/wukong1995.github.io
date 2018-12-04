---
  title: transform-carousel
  date: 2016-04-24 11:38:20
  articleTitle: CSS3 transform实现图片旋转木马3D浏览效果
  tags: ['css']
  categories: ['前端', 'css']
  description: CSS3 transform实现图片旋转木马3D浏览效果
---

首先DOM结构： 舞台>容器>图片
为舞台设置样式： `perspective: 800px;`
为容器设置样式：`transform-style: preserve-3d;position: relative;transition: transform .8s;`
元素本身style属性设置为`transform: rotateY(0deg);`为后面容器旋转获得初始值
为图片设置样式：`position: absolute;`
为每个图片设置样式（此时又九张图片）：

```css
img:nth-child(1) { transform: rotateY(   0deg ) translateZ(195.839px); }
img:nth-child(2) { transform: rotateY(  40deg ) translateZ(195.839px); }
img:nth-child(3) { transform: rotateY(  80deg ) translateZ(195.839px); }
img:nth-child(4) { transform: rotateY( 120deg ) translateZ(195.839px); }
img:nth-child(5) { transform: rotateY( 160deg ) translateZ(195.839px); }
img:nth-child(6) { transform: rotateY( 200deg ) translateZ(195.839px); }
img:nth-child(7) { transform: rotateY( 240deg ) translateZ(195.839px); }
img:nth-child(8) { transform: rotateY( 280deg ) translateZ(195.839px); }
img:nth-child(9) { transform: rotateY( 320deg ) translateZ(195.839px); }
```
`rotateY`的值为`360/图片的个数`，`translateZ`的值为`（图片宽度/2）/ tan（rorateY/2）`
最后，在js代码中改变容器的`rotateY`的值：代码如下：

```js
var stage = document.getElementById("stage");
stage.addEventListener('click',function() {
  var rotateY = this.style.webkitTransform;
  // 由左向右：rotateY-40
  var newPos = parseInt(rotateY)-40; 
  this.style.webkitTransform = 'rotateY(' + newPos + 'deg)';
});
```
