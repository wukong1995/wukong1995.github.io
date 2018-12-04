---
  title: window.onload，body onload，document.onreadystatechange
  date: 2016-06-08 17:06:37
  tags: [javascript]
  categories: [前端, javascript]
  description:
---


1、window.onload    页面全部加载完成，甚至包括图片

2、body.onload 
 等doucment加载完成再加载相应的脚本

3、document.onreadystatechange  
 当页面加载状态改变的时候执行这个方法。

```js
document.onreadystatechange = subSomething;//当页面加载状态改变的时候执行这个方法.
function subSomething() {
  if(document.readyState == "complete"){ //当页面加载状态为完全结束时进入
    //你要做的操作。
  }
}
```
