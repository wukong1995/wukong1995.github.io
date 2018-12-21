---
title: 小程序web-view的实践
date: 2018-12-21 22:48:04
tags: [小程序]
categories: [小程序]
description: 如何在小程序里面使用web-view，如何从web-view中进入小程序的页面
---

因为小程序的标签有限，像iframe这样的标签不能在小程序里面很好的展示，于是只能选择使用web-view去展示小程序的内容，点击web-view中iframe期望跳到小程序中的某个页面。

根据小程序的文档来，小程序嵌入web-view是一个很简单的事情
```html
<web-view src="{{url}}"></web-view>
```

如何在页面中跳入小程序的页面，为了防止污染现有的页面，我选择新建一个页面，引入小程序的sdk，
```slim
- if browser.wechat?
  = javascript_include_tag 'https://res.wx.qq.com/open/js/jweixin-1.3.2.js'
// .....
wx.miniProgram.navigateTo({
  url
})
```

ok，成功！