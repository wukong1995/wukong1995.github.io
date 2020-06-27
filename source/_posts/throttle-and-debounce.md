---
title: 聊一聊throttle and debounce
date: 2020-06-27 13:37:23
tags: [javascript]
categories: [前端, javascript]
description: throttle and debounce
---

两个都可以针对高频操作进行性能优化

### debounce（防抖）
debounce表示一段时间后要执行某种操作，假如在执行之前又有了执行信号，就取消之前的延迟操作，产生一个新的延迟操作。

代码实现：
```js
function debounce(fn, wait) {
  let timeout = null;

  fucntion debounced() {
    clearTimeout(timeout)

    timeout = setTimeout(() => {
      fn.apply(this, arguments)
    }, wait)
  }

  return debounced
}
```

lodash的实现方式：[https://github.com/lodash/lodash/blob/master/debounce.js](https://github.com/lodash/lodash/blob/master/debounce.js)，它优先使用requestAnimationFrame

它将触发频繁的事件合成一次执行，例如input、keyup、keydown、resize等

### throttle（节流）
throttle表示每隔一段时间就执行某种操作

代码实现：
```js
function throttle(fn, wait) {
  let timeout = null
  let firstRun = true

  function throttled() {
    if (firstRun) {
      firstRun = false
      fn(arguments).bind(this)
    }

    if (timeout) {
      return
    }

    timeout = setTimeout(() => {
      clearTimeout(timeout)
      fn.apply(this, arguments)
    }, wait)
  }

  return throttled
}
```

lodash的throttle是基于debounce的

它可以在一段时间中固定触发的次数， 例如scroll、resize等


PS: angular已经有10了🤯

