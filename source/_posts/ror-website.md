---
title: 使用turbolinks和rails-ujs搭建website
date: 2019-02-21 20:48:08
tags: [rails]
categories: [rails]
description: 如何快速便捷的创建一个网站
---

这次开发的管理项目，比较简单，想选用一个比较快速而且简单的方式。于是我选择了turbolinks+ujs套装。

##### turbolinks
turbolinks的主要作用是在页面跳转时，不会存在白屏，看起来是一个spa，其原理是拦截所有的链接跳转，使用ajax得到页面的html，替换body，合并头部的js和css文件。其中你的js文件一定要放在head中，而不是body底部。如果你担心第一次加载页面，空白时间太长，可以给script加上defer属性。

在项目中，使用webpack去打包所有的js文件（没有分包啊），所以在每次“跳转”页面的时候，需要重新执行一遍js，这个时候需要监听turbolinks的事件:

```js
document.addEventListener('turbolinks:load', () => {
  // .....
});
```

##### ujs
对于rails的ujs，不得不称赞，真的很优秀。在页面中，你可以通过给元素增加data-remote="true"来指示这是需要ujs来处理的。那么如何处理ujs返回的错误呢？

```js
$(document).on('ajax:error', (event) => {
  // ...
});
```

最后的代码如下：
```js
export default () => {
  RailsUJS.start();
  turbolinks.start();
  ajaxFeedback(); // handle ajax

  document.addEventListener('turbolinks:load', () => {
    // xxxx
  });
```