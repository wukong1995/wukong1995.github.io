---
date: 2018-09-08 22:51:13
title: 多瞅了几眼react blog
tags: [react]
categories: [react]
description: 学习react忽略的东西
---

#### react的生命周期
![image](http://res.cloudinary.com/dwudaridr/image/upload/v1536418494/react%E7%BB%84%E4%BB%B6%E5%91%A8%E6%9C%9F_jwteiz.png)

#### 问答
1. ajax为什么不在componentWillMount进行，而在componentDidMount中进行？
ajax是一个异步操作，在render调用之前，不会在一步操作中返回数据，这意味着组件将使用default数据呈现至少一次；同时，在异步操作的callback中更新state不会触发re-render。另外，没有办法在等到返回数据后再去掉用render。
react文档建议到：componentDidMount会在组件挂载后立即调用，需要dom节点的初始化应放在这里。如果你想远程获得数据，这是实例化网络请求的好地方。在这个方法中，调用setState这个方法会触发额外的render，它保证会在同一时间刷新，意为：即使你在这个情况下中调用render两次，用户不会看到中间的状态。

2. componentWillReceiveProps会执行一次还是多次？
多次，所以你在这个使用这个方法时，需要比较current props和 next props。

3. setState是同步的还是异步的？
异步。setState会触发re-render，如果是同步的话，可能会导致浏览器无反应。异步是为了获得更好的UI体验和性能。

4. Component和PureComponent的区别？
PureComponent在shouldComponentUpdate中对props进行浅比较，而Component则进行深比较。

5. react的事件机制
react中的事件在真实DOM中是全部挂在以事件代理的方式挂在document上面，事件代理的机制是事件冒泡。这是在react的事件中使用原生的阻止事件冒泡不起作用的原因。

