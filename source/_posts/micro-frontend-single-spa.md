---
title: 微前端-single-spa探索
date: 2019-11-16 22:01:03
tags: [javascript]
categories: [前端, javascript]
description: 前端微服务 = 微前端
---

这个星期看了single-spa的整体思路和demo，差不多能解答上篇blog的问题了。

### 1. 如何匹配的路由
sigle-spa在这册子项目的时候，需要指定路由在何种情况下挂载项目，这是不是意味着最好以url的prefix来划分项目更好一些？

```js
import {registerApplication, start} from 'single-spa'
registerApplication(
  // Name of our single-spa application
  'home',
  // Our loading function
  () => import('./src/home/home.app.js'),
  // Our activity function
  () => location.pathname === "" || 
    location.pathname === "/" || 
    location.pathname.startsWith('/home')
);
start()
```

那么single-spa内部也是通过监听history的变化，进而判断如何挂载项目

```js
// navigation/navigation-event
// ...
// We will trigger an app change for any routing events.
window.addEventListener('hashchange', urlReroute);
window.addEventListener('popstate', urlReroute);
```

另外single-spa为需要项目之间跳转的a标签，暴露了一个`navigateToUrl`的方法，我想了一下如果需要在js里面去进行项目之间的跳转，就是用最原始的方法：

```js
window.history.pushState(null, null, url);
```

### 2. 多个项目如何通信
single-spa并不提供这个功能，但在github上看到了一个思路：(https://github.com/me-12/single-spa-portal-example)[https://github.com/me-12/single-spa-portal-example]

这个项目只要的思想是：在进行registerApplication的时候，主项目也会为每个子项目挂载一个store，并为每个子项目通过customProps将dispatch传递下去，那么dispatch主要做的就是将每个子项目的每次dispatch都会分发给每个子项目，子项目收到后，各自做自己的事情。

```js
export class GlobalEventDistributor {

    constructor() {
        this.stores = [];
    }

    registerStore(store) {
        this.stores.push(store);
    }

    dispatch(event) {
        this.stores.forEach((s) => s.dispatch(event));
    }
}
```

### 3. 打包时，如何将多个子项目相同包合并
这个是不正确的思路，如果每次打包的时候，都需要查找每个子项目的相同包，假如这个“相同”发生变化，那么就意味着主项目中打包出来的公共包需要改变，这是不正确的。微服务必须要尽可能的保持主项目即容器稳定，而不是每次迭代都有更改的可能。所以解决办法是公共包需要手动维护，并且更新的可能性最小。

对于打包这个功能，同样single-spa也不提供这个功能，但官网的文档上提供了三种解决方法：(https://single-spa.js.org/docs/separating-applications)[https://single-spa.js.org/docs/separating-applications]

比较靠谱的是第三个，为此，创建一个manifest文件，该文件在单Spa应用程序的部署过程中进行更新，该manifest控制“ SPA”哪些版本处于“活动”状态。然后根据manifest更改加载哪个javascript文件。

这个可以做到更新单个子项目，而不需要更新其他项目的目的。

暂未实施，这个做为下一步的重点关注。

