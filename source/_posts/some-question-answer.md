---
title: 前端相关的问题解答
date: 2019-05-01 15:03:57
tags: [总结, 前端]
categories: [总结, 前端]
description: 最近在公众号上看到一篇文章里面，里面的问题我也有些不明白的地方...
---

最近在公众号上看到一篇文章里面，虽然是前端的米啊是，但是里面涉及到的问题众多。[链接在这里](https://mp.weixin.qq.com/s/wQjg9XwMubwaGCj64BH_JA)

#### redux的工作流程
>component --> dispatch(action) --> reducer --> subscribe --> getState --> component

#### react 16新增内容
1. ReactDOM.createPortal
2. 新的生命周期
3. hooks
4. context
5. fragment

其实印象最深的是hooks，其他的新增也应该关注，最直接的方法是看react blog。

#### 四次挥手详细介绍
[参考链接](https://juejin.im/post/5a0444d45188255ea95b66bc)

#### TCP 有哪些手段保证可靠交付
* 校验和
* 序列号
* 确认应答
* 超时重传
* 连接管理
* 流量控制
* 拥塞控制

#### 如何预防中间人攻击
[参考链接](https://www.zhihu.com/question/20744215)

#### DNS 解析会出错吗，为什么
[参考链接](https://blog.csdn.net/crfoxzl/article/details/2069206)

#### ES6 的 Set 内部实现
[参考链接](https://github.com/mqyqingfeng/Blog/issues/91)

#### 如何应对流量劫持
[参考链接](https://www.zhihu.com/question/35720092)

#### 算法：top-K 问题
我是第一次听说这个问题，问题描述是：给定n个整数，求其前k大或前k小的问题，简称TOP-K问题

#### webpack 的 plugins 和 loaders 的实现原理
[参考链接](https://juejin.im/entry/5b0e3eba5188251534379615)
[参考链接](https://juejin.im/post/5bbf190de51d450ea52fffd3)

#### webpack 如何优化编译速度
这个我能想到多线程编译
[参考链接](https://juejin.im/entry/5c302140f265da611b587f99)

#### 事件循环机制，node 和浏览器的事件循环机制区别
[参考链接](https://wukong1995.github.io/2019/04/11/translate-javascript-event-loop-explained/)

区别的话我会选择node和浏览器不同进行理解，例如node没有browser api

[参考链接](https://juejin.im/post/5c337ae06fb9a049bc4cd218)

#### rxjs 高阶数据流定义，常用高阶数据流操作符? RxJS 冷热流区别， RxJS 调试方法，RxJS 相对于其他状态管理方案的优势？
我第一次看到这个东西，[官网在这里](https://rxjs-dev.firebaseapp.com/)，前端真的是任重而道远！

#### nginx 负载均衡配置
[参考链接](https://blog.csdn.net/xyang81/article/details/51702900)
我服务器上的nginx还没配置好=。=

#### JWT 优缺点
感谢开发小程序中，让我认识到JWT，这里又想起了一个新的概念“单点登录”

[参考链接](https://www.guonanjun.com/220.html)

#### 针对 React 的性能优化手段
[参考链接](https://zhuanlan.zhihu.com/p/37148975)

#### forceUpdate 经历了哪些生命周期，子组件呢?
[参考链接](https://stackoverflow.com/questions/30626030/can-you-force-a-react-component-to-rerender-without-calling-setstate)

#### 谈谈 XSS 防御，以及 Content-Security-Policy 细节
[参考链接](http://qingbob.com/Excess-XSS/)

[参考链接](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))

#### canvas 优化绘制性能
[参考链接](http://taobaofed.org/blog/2016/02/22/canvas-performance/)

#### 谈谈 css 预处理器机制
[参考链接](https://github.com/cssmagic/blog/issues/73)

#### ssr 性能优化，node 中间层细节处理
现在越来越多的公司将node作为中间层去处理数据，转发数据。
[参考链接](https://segmentfault.com/a/1190000016722457)

#### 发布订阅模式和观察者模式
观察者模式维护一个observers，每当有变化，就通知观察者。
发布订阅则是由一个中介再做这件事。

#### PS：写在最后的话
跨域、性能优化应该是一个都关注的点，另外webpack也是一个关注点，但是我只停留在会用会配置的阶段=。=

整理起来真的感觉又杂又多，感觉自己已经很努力的学习前端的知识，但是新的东西越来越多的向你涌来。最近看了web性能优化，给了我很大的启示：自己对有些问题的理解只停留在表面，eg：使用雪碧图的目的是减少http请求，但从没想过为什么要减少http的请求。前端其实涉及到知识点众多，而不仅仅是html、js、css，学有所思才是王道。

