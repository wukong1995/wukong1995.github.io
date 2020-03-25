---
title: 现有项目接入single-spa，浏览器一直处于url重定向的状态==
date: 2020-03-25 15:58:27
tags: [前端]
categories: [前端]
description: 死循环，浏览器挂掉，多整几次，电脑就死掉了😄
---

在项目中接入single-spa后，有一个菜单，只要点到它就直接死掉了，刚开始怀疑是点击的时候，右手动的重定向，检查了代码，发现没这个操作。找了组件里面有一行“阻止浏览器”后退的代码

```js
// 加载后绑定Window的路由事件
  componentDidMount() {
    if (window.history && window.history.pushState) {
      window.addEventListener('popstate', this.stopGoBack)
      window.history.pushState('forward', null, '#')
      window.history.forward(1)
    }
  }

  // 销毁前清除监听Window的路由事件
  componentWillUnmount() {
    window.removeEventListener('popstate', this.stopGoBack)
  }
  // --- 生命周期函数区 End ---

  // --- 阻止回退 ---
  stopGoBack = () => {
    // 业务代码
    window.history.pushState('forward', null, '#')
    window.history.forward(1)
  }
```

找了popstate的解释：
> 当活动历史记录条目更改时，将触发popstate事件。如果被激活的历史记录条目是通过对history.pushState（）的调用创建的，或者受到对history.replaceState（）的调用的影响，popstate事件的state属性包含历史条目的状态对象的副本。

> 需要注意的是调用history.pushState()或history.replaceState()不会触发popstate事件。只有在做出浏览器动作时，才会触发该事件，如用户点击浏览器的回退按钮（或者在Javascript代码中调用history.back()或者history.forward()方法）

>不同的浏览器在加载页面时处理popstate事件的形式存在差异。页面加载时Chrome和Safari通常会触发(emit )popstate事件，但Firefox则不会。

在react-router中的BrowserHistory，在window.history的包装的。项目单独运行时，可以正常使用，接入single-spa后，就不行了，于是乎看了single-spa的代码

```js
// navigation-events.js
window.history.pushState = patchedUpdateState(window.history.pushState);
window.history.replaceState = patchedUpdateState(window.history.replaceState);

function patchedUpdateState(updateState) {
    return function() {
      const urlBefore = window.location.href;
      const result = updateState.apply(this, arguments);
      const urlAfter = window.location.href;

      if (!urlRerouteOnly || urlBefore !== urlAfter) {
        urlReroute(createPopStateEvent(window.history.state));
      }

      return result;
    };
}

function createPopStateEvent(state) {
    // https://github.com/single-spa/single-spa/issues/224 and https://github.com/single-spa/single-spa-angular/issues/49
    // We need a popstate event even though the browser doesn't do one by default when you call replaceState, so that
    // all the applications can reroute.
    try {
      return new PopStateEvent("popstate", { state });
    } catch (err) {
      // IE 11 compatibility https://github.com/single-spa/single-spa/issues/299
      // https://docs.microsoft.com/en-us/openspecs/ie_standards/ms-html5e/bd560f47-b349-4d2c-baa8-f1560fb489dd
      const evt = document.createEvent("PopStateEvent");
      evt.initPopStateEvent("popstate", false, false, state);
      return evt;
    }
}

export function navigateToUrl(obj) {
  let url;
  if (typeof obj === "string") {
    url = obj;
  } else if (this && this.href) {
    url = this.href;
  } else if (
    obj &&
    obj.currentTarget &&
    obj.currentTarget.href &&
    obj.preventDefault
  ) {
    url = obj.currentTarget.href;
    obj.preventDefault();
  } else {
    throw Error(
      formatErrorMessage(
        14,
        __DEV__ &&
          `singleSpaNavigate/navigateToUrl must be either called with a string url, with an <a> tag as its context, or with an event whose currentTarget is an <a> tag`
      )
    );
  }

  const current = parseUri(window.location.href);
  const destination = parseUri(url);

  if (url.indexOf("#") === 0) {
    window.location.hash = destination.hash;
  } else if (current.host !== destination.host && destination.host) {
    if (process.env.BABEL_ENV === "test") {
      return { wouldHaveReloadedThePage: true };
    } else {
      window.location.href = url;
    }
  } else if (
    destination.pathname === current.pathname &&
    destination.search === current.pathname
  ) {
    window.location.hash = destination.hash;
  } else {
    // different path, host, or query params
    window.history.pushState(null, null, url);
  }
}
```

那么popState是有在操作浏览器的前进后退才会触发，那single-spa是如何监控页面变化的呢？

single-spa中跳转url给的api是navigateToUrl，它是基于history h5的api。想象一下，react-route人中提供的NavLink和Link应该也是基于pushState或者replaceState的。

所有的跳转操作都是基于pushState或replaceState的，single-spa就对history的pushState和replaceState进行了重写，在patchedUpdateState中判断url的变化，如果url变化，就手动触发popstate事件。

那么最上面的代码是如何导致死循环的呢？

```js
window.addEventListener('popstate', this.stopGoBack) // 监听popstate

stopGoBack = () => {
    // pushState已经被重写，这里pushState后，就执行了patchedUpdateState，在patchedUpdateState中又手动触发popstate的事件，于是乎就成了自己掉自己
    window.history.pushState('forward', null, '#')
    window.history.forward(1)
}
```

-----
走到这，我又尝试在react的spa应用中执行window.history.pushState(null, null, url)，期待着能跳转页面，btw，url变了，但是组件挂载没有发生变化，于是乎，又去找了[history的代码](https://github.com/ReactTraining/history/blob/master/modules/createBrowserHistory.js#L238)，能看出来createBrowserHistory中内部保存着一个history的状态，react-route接收根据history.location的值来进行match，[match成功后挂载组件](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/modules/Switch.js#L40)，并不是直接使用的window.history上的值进行判断的，所以在控制台中执行，地址栏会改变，但是组件挂载不会发生变化。
