---
title: 新的一年的第一周get了
date: 2019-02-17 12:52:48
tags: [总结]
categories: [总结]
description: 新的一年的第一周get到些什么
---

##### 1. 如何创建xhr拦截器
override xhr的open方法[参考链接](https://medium.com/@gilfink/quick-tip-creating-an-xmlhttprequest-interceptor-1da23cf90b76)

```js
let oldXHROpen = window.XMLHttpRequest.prototype.open;
window.XMLHttpRequest.prototype.open = function(method, url, async, user, password) {
  // do something with the method, url and etc.
  this.addEventListener('load', function() {
    // do something with the response text
    console.log('load: ' + this.responseText);
  });

 return oldXHROpen.apply(this, arguments);
}
```

##### 2. 当参数为object对象时，如果使用默认参数，怎么做？

```js
const spin = ({color: 'red'}) => {}

// use default params
spin();    // error
span({});  // use dedault params

const spin1 = ({color: 'red'} = {}) => {}
spin1();    // use dedault params
span1({});  // use dedault params
```

##### 3. 布局是[x-y]------[z]时
参考ruby-china中header的样式
z
通常使用flex布局，将x和y作为一部分
但可以看成类似这样的布局[x]-[y]------[z]，y的style为`margin-right: auto;`

##### 4. 如何提交react的性能
[参考链接](https://marmelab.com/blog/2017/02/06/react-is-slow-react-is-fast.html)
* 减少不必要的渲染[why-did-you-update](https://github.com/maicki/why-did-you-update)
* 切割组件
* [Recompose](https://github.com/acdlite/recompose)
* [Reselect](https://github.com/reduxjs/reselect)
* Beware of Object Literals in JSX
```js
<Cropper
 style={{ maxHeight: 300 }}
/>
```
上面的code中的`{ maxHeight: 300 }`在每一次render过程中都是一个新的对象，所以`Cropper`的render每次都会被执行，可以改成
```js
const style = { maxHeight: 300 };
<Cropper
 style={style}
/>
```
