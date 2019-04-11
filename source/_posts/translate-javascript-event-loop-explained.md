---
title: 翻译：理解js中的event loop
date: 2019-04-11 20:44:05
tags: [翻译, javascript]
categories: [翻译]
description: javascript event loop explained
---

之前看的文，感觉不太完整，于是找了篇详细的文章。[原文链接](https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4)

Javascript是如何异步和单线程的？简短的回答是javascript语言是单线程的，异步行为不是它的一部分，相反，它是建立在浏览器（或编程环境）中的核心JavaScript语言之上，并通过浏览器API访问。

现在为了得到答案，让我写两个示例代码片段。

### 基本架构
![image](http://res.cloudinary.com/dwudaridr/image/upload/v1533706275/blog/translate-javascript-event-loop-explained-1.png)

+ **堆**：对象在堆中分配，表示大多数非结构化的内存区域。
+ **栈**：这表示JavaScript代码执行提供的单个线程，函数调用形成一个栈。
+ **浏览器或Web API**是内置在你的web浏览器中，能够暴露浏览器和周围的计算机环境中的数据，并使用它执行游泳的复杂操作。它们不是JavaScript语言的一部分，而是基于核心JavaScript语言构建，为你的JavaScript代码中提供额外的超能力。例如，*[Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)*提供了一些简单的JavaScript构造，用于检索数据，所以你可以说，在`Google map`上绘制你的位置。在后台，浏览器实际上使用一些复杂的低级代码（例如C++）与设备的GPS硬件（或者任何可用于确定位置数据的信息）进行通信，检索位置数据，并将其返回到浏览器环境用来使用在你的代码中。但同样，这种复杂性是由API抽象出来的。

#### 代码片段1:迷惑心灵
```js
function main(){
  console.log('A');
  setTimeout(
    function display(){ console.log('B'); }
  ,0);
  console.log('C');
}
main();
//  Output
//  A
//  C
//  B
```
这里，我们有一个主函数，它有两个`console.log`：将A和C输入到控制台上。它们中间是一个在0ms后将B输出到控制台上的`setTimeOut`调用。

![image](http://res.cloudinary.com/dwudaridr/image/upload/v1533706275/blog/translate-javascript-event-loop-explained-2.png)
>

1. 调用主函数，首先将它推入到栈中。然后浏览器将主函数的第一个声明`console.log('A')`推入到栈中，执行此语句，并在完成后弹出，字母A显示在控制台上。
2. 第二个声明`setTimeout`推入栈中并开始执行。`setTimeout`函数使用浏览器的API来延迟回调其中的函数。一旦交接给brower完成timer，这一帧就被推出。
3. 当计时器在浏览器中运行来回调`exec`函数时，`console.log(‘C’)`被推入栈中。在这种特殊的情况下，由于提供的延迟是0ms，一旦浏览器接收到它（理想情况下），回调将被添加到消息队列中。
4. 执行完主函数的最后一个语句，主函数从调用堆栈中弹出，从而堆栈为空。对于浏览器将任何消息从队列推送到调用堆栈中，调用堆栈必须为空。这就是为什么即使`setTimeout`中提供的延迟是0ms，`exec`的会带哦耶必须等到调用堆栈中的所有帧的执行完成。
5. 现在`exec`的回调被推入调用栈，接着执行。字母C显示在控制台上。这就是JavaScript的事件循环。

>因此setTimeout（function，delayTime）中的delay参数不代表执行函数之后的精确时间延迟。它代表最小等待时间，之后在某个时间点执行该功能。

#### 代码片段2:深入理解
```js
function main(){
  console.log('A');
  setTimeout(
    function exec(){ console.log('B'); }
  , 0);
  runWhileLoopForNSeconds(3);
  console.log('C');
}
main();
function runWhileLoopForNSeconds(sec){
  let start = Date.now(), now = start;
  while (now - start < (sec*1000)) {
    now = Date.now();
  }
}
// Output
// A
// C
// B
```
![image](http://res.cloudinary.com/dwudaridr/image/upload/v1533706275/blog/translate-javascript-event-loop-explained-3.png)

+ 函数`runWhileLoopF​​orNSeconds`完全符合其名称所代表的含义，它会不断检查从调用时间开始经过的时间是否等于函数参数提供的秒数。要记住的要点是`while`循环（与许多其他循环一样）是一个阻塞语句，意味着它的执行发生在调用堆栈上，并且不使用浏览器API。因此它会阻止所有后续语句，知道它执行完成。
+ 所以在上面的代码中，即使setTimeout的延迟为0ms且while循环运行3s，exec()回调也会卡在消息队列中。while循环继续在调用堆栈（单线程）上运行，直到3s已经过去。在调用堆栈变空之后，回调exec()被移动到调用堆栈并执行。
+ 因此`setTimeout()`中的delay参数不保证在计时器完成延迟后开始执行。它是延迟部分的最短时间。
