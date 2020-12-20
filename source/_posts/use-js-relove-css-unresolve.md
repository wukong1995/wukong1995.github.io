---
title: css解决不了的，就用js解决
date: 2020-12-20 16:31:48
tags: [css]
categories: [前端, css]
description: js大法好
---

对于展示上的问题，我一向是css能解决的就不要动用js。这个迭代中加了一个放大缩小的功能，放大的话势必要出现横向滚动条，但是在实际上还会有超宽的absolute元素，我写了一个简单的demo

```html
<style>
  .container {
    width: 400px;
    height: 600px;
    border: 1px solid rgb(198, 198, 199);
    margin: 50px auto;
    overflow-y: scroll;
    overflow-x: auto;
  }

  .inner {
    position: relative;
    border: 1px solid blue;
  }

  .mark {
    position: absolute;
    left: 120px;
    top: 120px;
    width: 600px;
    height: 600px;
    border: 2px solid red;
  }

  img {
    width: 120%;
  }
</style>

<div class="container">
  <div class="inner">
    <img src="https://aibici-test.oss-cn-beijing.aliyuncs.com/document-mining-backend/c9325f71bc7df09b10de105835314c2e.jpg" />
    <div class="mark"></div>
  </div>
</div>
```

mask超级宽，一旦它超出父元素的宽度是，势必会让父元素变宽。我想要的效果是mask不会影响父元素的宽度。于是我开始在这个基础上各种魔改，以达到我的效果。结果折腾了大半天也不可以。

最后是使用js会动态计算inner的宽高。同时我反思了一下这个css方案的失败。img的宽度使用百分比的时候，它依赖于inner的宽度，如果我需要hidden absolute元素的宽度，必须在inner上加overflow: hidden。这样一来，图片超出100%的时候，也是无法滚动的。hidde和scroll互相矛盾；如果给mark加一个父元素testtest来包裹，有一个问题是testtest使用absolute定位式，宽度是等于inner的宽度的，即是inneryou了scroll。

所以最后的方案是使用js去计算inner的宽度，img永远保持width为100%
