---
title: refuse-to-overuse-react
date: 2018-11-05 19:09:38
articleTitle: 拒绝过度使用react
tags: ['react', 'js']
categories: ['react', 'js']
---

对于目前三大潮流框架，我先使用的是Angular，用它写了一个购物车，感觉非常好。工作之后，使用的react，用顺手之后，第一解决办法就是想到react，导致有很多过度使用的地方。

这个在原来的基础上，需要做一套和之前的东西功能上完全不一样的东西，于是我暗暗下定决心，尽量不使用react和其它的package...

对于搜索这一块，之前使用react做的，这一次我想要尝试一下使用js去实现...缕了一下，我需要做的

```js
// 请求数据（改变“查看更多”按钮的状态）
// 处理数据（根据数据生成html片段）
// 渲染dom（将html插入target dom）
```

#### 请求数据
在react 中，通过改变state的状态，js中需要操作button

```js
// react
this.setState({
  isFetch: status
});

const Button = (status) => {
  return (
    <button type="button">{status ? '加载中' : '查看更多'}</button>
  )
}

// js
$button.html(getButtonHTML(status));

const getButtonHTML = (status) => {
  return status ? '加载中' : '查看更多'
}
```

#### 处理数据
在react中通过改变state去实现dom的动态插入，react中是写一个组件，js中为了清晰期间，我将每个html片段分成一个文件

```js
// react
const List = (list) => {
  return (
    list.map(item => <p>{item.title}</p>)
  )
}

// js
const list = (list) => {
  return list.reduce((acc, item) => `${acc}<p>${item.title}<p>`)
}
```

#### 渲染dom
react是通过改变status，js是将生成的html fragment插入
```js
// react
this.setStatus({
  list: [...oldList, ...newList]
})

// js
$container.append(htmlFragment);
```

#### 总结
使用react给我带来的最大感受就是一切组件化，一个问题大变小，是非常容易解决的，组件化可以代码更smart...大胆的去除code的bad smell

明天也是元气满满的一天加油:)





