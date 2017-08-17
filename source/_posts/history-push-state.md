---
title: history-push-state
date: 2018-07-13 18:48:07
articleTitle: SPA起点？
tags: ['javascript', history']
description: use history
categories: ['前端']
---

如何想要点击浏览器自带的前进 后退按钮 不刷新页面，可以通过改变history的state
想了一下，写了一个小demo。
```javascript
<nav>
  <a href="/home">主页</a>
  <a href="/about">个人</a>
  <a href="/test">测试</a>
</nav>
<div id="content"></div>
<script>
  const baseHref = location.href;
  const baseUrl = '/test.html';

  window.onload = () => {
    const links = document.querySelectorAll('a');

    links.forEach(item => {
      item.addEventListener('click', (event) => {
        event.preventDefault();

        const href = item.getAttribute('href');
        history.pushState('', '', baseHref + href);
       setContentHtml(href);
      });
    });

    window.onpopstate = (event) => {
      // console.log(event);
      const currentPathname = event.target.location.pathname;
      const targetHref = currentPathname.split(baseUrl)[1];
      setContentHtml(targetHref);
    }

    const setContentHtml = (content) => {
      document.getElementById('content').innerHTML = content;
    }

    // 接管路由
    // 1. 得到baseUrl之后的href
    // 2. 根据href去render
  }
</script>
```
假如你是直接打开html文件，使用`history.pushState`会出错，这时候你需要新开一个server。
其中最主要的是，你要托管a链接的herf。
其中在`onpopstate`中，我得到`targetHref`的方式，感觉有些蠢，目前还没想到好的方法。
去看一下其他的框架可能会给我一个不一样的思路。
飘过～

#### 2018-07-14 更新
在history这个项目中得到的启示：
`history.pushState`的第一个参数就是`onpopstate`事件中的`event.state`
这样的话，我可以对现有的进行一些改进
```javascript
// first argumens is event state
// history.pushState('', '', baseHref + href);
history.pushState(href, null, baseHref + href);

// ....

// const currentPathname = event.target.location.pathname;
// const targetHref = currentPathname.split(baseUrl)[1];
const targetHref = event.state;
```
这样想一下，`event.state`里面可以存很多东西....
