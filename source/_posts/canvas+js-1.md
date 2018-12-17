---
title: 利用Canvas+js实现贪吃蛇(1)
date: 2016-06-12 21:04:29
tags: [javascript, demo]
categories: [前端, javascript]
description:
---

今天想写一个贪吃蛇的页面，于是就选择了Canvs，不过只实现蛇身的长度是1的情况，对于蛇身大于2的情况，我还没想出来 如何绘制蛇，等查资料后，再来实现
下面是源码部分：

```html
<div style=" text-align: center">
  <canvas id="myCanvas" width="630" height="630"></canvas>
</div>
<div style="position: absolute;top: 300px;left: 30px;">
  score:<span id="score">0</span>
</div>
```

```ks
window.onload = function() {
  var canvas = document.getElementById('myCanvas'),
      span_score = document.getElementById('score'),
      context = canvas.getContext('2d'),
      maxNumber = 630,
      stride = 15,
      x = 31,
      y = 61,
      SnakeLen = 1,
      speed = 500,
      direct = 37,
      food_x = Math.ceil(Math.random() * 40 + 2) * 15 + 1,
      food_y = Math.ceil(Math.random() * 40 + 2) * 15 + 1,
      score = 0,
      timer;

  init();
  update();

  document.onkeydown=function(event) {
      clearInterval(timer)
      direct = event.keyCode;
      update();
  }
  // 定时更新画面
  function update() {
      timer = setInterval(function() {
          var _x = x,
              _y = y;
          // 37:左 38:上 39:右  40:下
          switch (direct) {
              case 37:
                  x = x - stride;
                  break;
              case 38:
                  y = y - stride;
                  break;
              case 39:
                  x = x + stride;
                  break;
              case 40:
                   y = y + stride;
                  break;
              default:
                  x = x - stride;
                  break;
          }
          // 到达边界
          if(x < 1 || y < 1 || x > maxNumber || y > maxNumber) {
              clearInterval(timer);
              alert("Game over!");
          } else {
              // 擦除贪吃蛇
              clear(_x,_y);
              // 重新绘制贪吃蛇
              draw(x,y);

              // 检测是否吃到食物
              checkFood();
          }
      },speed)
  }

  // 绘制贪吃蛇
  function drawSnake() {
      //
  }

  // 检测是否吃到食物
  function checkFood() {
      if(x === food_x && y === food_y) {
          food_x = Math.ceil(Math.random() * 40 + 2) * 15 + 1;
          food_y = Math.ceil(Math.random() * 40 + 2) * 15 + 1;
          draw(food_x,food_y);
          SnakeLen++;
          score += 10;
          span_score.innerHTML = score;
      }
  }

  // 清除
  function clear(x,y) {
      context.clearRect(x,y,stride - 2,stride - 2);
  }
  // 画食物
  function draw(x,y) {
      context.fillRect(x,y,stride - 2,stride - 2);
  }
  // 初始化函数
  function init() {
      context.lineWidth = 1;
      context.fillStyle = '#69D69E';
      context.strokeStyle="#89EFA3";
      var i = 0;
      while(i <= maxNumber) {
          context.moveTo(0, i);
          context.lineTo(maxNumber,i);
          context.stroke();

          context.moveTo(i,0);
          context.lineTo(i,maxNumber);
          context.stroke();
          i += stride;
      }
      draw(food_x,food_y);
      console.log("foodx"+food_x+"foody"+food_y);
      draw(x,y);
  }
}
```
其中，drawSnake函数是实现蛇身长度大于2时，实现的，很可惜，现在还未实现，里面有很多多余配置参数，等改进后，就能用到了。


在开始时，我将小方块的设置为15*15，绘制的小蛇方块也是15*15，在每次小蛇运动时，就会把网格线顺便擦除，所以刚开始在draw函数调用init函数（在刚开始时），想重新绘制网格线，发现，小方块运动的速度很慢，这是是说 这种方法不可取，每次重新绘制网格线，花费的时间太多，
在update函数中，一开始，就先擦除小蛇，再判断小蛇是否到达边界，假如小蛇到达边界，虽然游戏结束，但是小蛇不减了，因为，小蛇被我擦除了，小蛇的新位置超出了边界，绘制出来，也不会出现在  画布上，我感觉 这个结果 不太符合常理，所以就使用了两个临时变量保存，这样解决了问题，花费的资源就多了。。。不知道还有什么其他的好办法。。

PS：所有测试都是在最新版本谷歌浏览器下进行


