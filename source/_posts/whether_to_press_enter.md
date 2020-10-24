---
title: 你是否按下enter键
date: 2020-10-10 21:31:50
tags: [javascript]
categories: [javascript]
description:
---

今天在查看代码的时候，判断按下的键是否是enter，有两个不同的方式，有监听keypress的，也有监听keydown的，判断键是否是enter有使用event.which的，也有使用event.keyCode的，它们之前又有什么区别呢？

```js
<Input
 onKeyPress={keyPress}
  onKeyDown={keyDown}
/>
```

keypress event的定义
>The keypress event is fired when a key that produces a character value is pressed down. Examples of keys that produce a character value are alphabetic, numeric, and punctuation keys. Examples of keys that don't produce a character value are modifier keys such as Alt, Shift, Ctrl, or Meta.

keydown event的定义
>The keydown event is fired when a key is pressed.
>Unlike the keypress event, the keydown event is fired for all keys, regardless of whether they produce a character value.

所以两者的区别是按下修饰键不回出发keypress事件。所以对于enter键的判断使用两个事件进行捕捉都是可以的，但是keypress已经不推荐使用。

另外，两者的第一个参数都是[keyEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)，在mdn的解释中，keyCode和which作用一样，定义都是
>Returns a Number representing a system and implementation dependent numeric code identifying the unmodified value of the pressed key

不过这两个方法也是不推荐使用了，MDN推荐使用`KeyboardEvent.key` 来代替，不过这个属性会返回string类型，而keyCode和which都返回number类型。怎么说，有`Don't break the web`这个理念在这里，只能说你还可以用下去。


我拿着`keyDown`去react里面搜了一下，发现了一个`domEvents.js`的文件，里面全都是创建一些常见类型的事件。关于react中的合成事件可以点[这里](https://reactjs.org/docs/events.html)








