---
title: 利用Canvas+js实现贪吃蛇(3)
date: 2016-06-23 14:29:25
tags: [javascript, demo]
categories: [前端, javascript]
description:
---


想了想，蛇向下走的时候按上键，如果直接将蛇头变蛇身，好像不符合常理，所以处理这种情况，应当做蛇头碰到蛇身。。
另外，实现蛇头碰蛇身的功能
界面就不美化了。。。
下一步就是封装成面向对象的插件了。。。

```js
window.onload = function() {
  var canvas = document.getElementById('myCanvas'),
      span_score = document.getElementById('score'),
      context = canvas.getContext('2d'),
      maxNumber = 630,   // 最大长宽，这里的画布是正方形
      stride = 15,  //每个小方块的长宽，
      snakeLength = 1,  //蛇身长度
      snake = new Array(),  //蛇身数组
      speed = 300,   // 移动速度
      direct = 37,   //移动方向
      food_x = 31,   //食物的坐标   可以改为object对象
      food_y = 121,
      score = 0,   // 得分
      timer;       //setInterval

  init();
  update();

  document.onkeydown=function(event) {
      clearInterval(timer);
      // 37:左 38:上 39:右  40:下
      //  改为以下 内容 ，处理反方向的bug
      var newDir = (event.keyCode > 36 && event.keyCode < 41) ? event.keyCode : 40;
      // 处理反方向
      if(Math.abs(direct - newDir) == 2) {
          alert("头碰到身体了，Game over!");
      } else {
          direct = newDir;
          update();
      }
  }
  // 随机生成食物
  function randFood() {
      do{
          food_x = Math.ceil(Math.random() * 40 + 2) * 15 + 1;
          food_y = Math.ceil(Math.random() * 40 + 2) * 15 + 1;
      }while(isBody(food_x,food_y));

  }
  // 判断食物是否在蛇的身体上
  function isBody(param_x,param_y) {
      // 判断蛇头是否碰到了蛇身
      for (var i = 0; i < snakeLength - 1; i++) {
          if(snake[i].x === param_x && snake[i].y === param_y) {
              return true;
          }
      }
      // 判断食物是否在蛇的身体上
      var snakeLast = snake[snakeLength-1];
      if(snakeLast) {
          if(snakeLast.x === param_x && snakeLast.y === param_y) {
              return true;
          }
      }
      return false;
  }
  // 定时更新画面
  function update() {
      timer = setInterval(function() {
          // 到达边界
          if(snake[0].x < 1 || snake[0].y < 1 || snake[0].x > maxNumber || snake[0].y > maxNumber) {
              clearInterval(timer);
              alert('出边界了，Game over!');
          } else {
              // 重新绘制贪吃蛇
              drawSnake();

              // 检测是否吃到食物
              checkFood();
          }
      },speed)
  }

  // 绘制贪吃蛇
  function drawSnake() {
      // 擦出蛇尾巴
      var snakeLast = snake[snakeLength-1],
          snakeHead = snake[0];
      clear(snakeLast.x,snakeLast.y);

      // 删除尾巴
      snake.pop();
      // 增加头
      addHead(snakeHead.x,snakeHead.y);
  }
  // 增加头
  function addHead(param_x,param_y) {
      var newX,newY;
      // 37:左 38:上 39:右  40:下
      switch (direct) {
          case 37:
              newX = param_x - stride;
              newY = param_y;
              break;
          case 38:
              newY = param_y - stride;
              newX = param_x;
              break;
          case 39:
              newX = param_x + stride;
              newY = param_y;
              break;
          case 40:
              newY = param_y + stride;
              newX = param_x;
              break;
      }

      // 增加蛇头
      // 检测头是否 碰到身体
      if(snakeLength > 3 && isBody(newX,newY)) {
          clearInterval(timer);
          alert("头碰到身体了，Game over!");
      } else {
          snake.unshift({x:newX,y:newY});
          draw(snake[0].x,snake[0].y);
      }
  }

  // 检测是否吃到食物
  function checkFood() {
      if(snake[0].x === food_x && snake[0].y === food_y) {
          // 增加头
          snakeLength += 1;
          addHead(food_x,food_y);
          // 改变分数
          score += 10;
          span_score.innerHTML = score;
          // 改变速度
          speed = (speed > 100) ? (speed - 20) : speed;
          randFood();
          draw(food_x,food_y);
      }
  }

  // 清除
  function clear(param_x,param_y) {
      context.clearRect(param_x,param_y,stride - 2,stride - 2);
  }
  // 画小方块
  function draw(param_x,param_y) {
      context.fillRect(param_x,param_y,stride - 2,stride - 2);
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
      snake.push({x:31,y:61});
      randFood();
      draw(food_x,food_y);
      draw(snake[0].x,snake[0].y);
  }
}


ps：所有测试都是在最新版本的谷歌浏览器下进行




