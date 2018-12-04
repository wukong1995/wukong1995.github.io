---
date: 2017-07-23 20:40:35
tags: [angularjs, react, graphql]
categories: [graphql]
title: 使用relay和apollo的感觉
description: 这个主要是叙述relay和Apollo
---

### 使用relay的感受

初次接触relay，感觉上很臃肿，因为必须为每个组件设置container；若query层级嵌套很深，为了组件化，就必须将每一个react的component全部分开，在项目中，我写了六个组件，那么每个组件都需要container，写起来感觉满满的恶意。

### 使用Apollo

再次接触Apollo，看了文档，感觉和relay大同小异，与relay的不同是：无需为每个component设置container，最后写一个query就ok👌了。query也可以由多个fragment组成。另外，apollo也为angularjs提供了解决方案，有点想不明白🤔，angularjs本身就是双向数据绑定，为什么要对它提供解决方案...另外，Apollo虽然自身内部集成了redux，假如你的项目中使用了redux，你可以使用redux而不用Apollo内部的redux。

relay 因为container的存在，数据划分的比较严密，你只能在当前的container访问fragment中的的属性，不能访问父或子fragment的属性。而apollo直接使用的是请求得到的Object...
Apollo也可以和项目原有的redux相结合使用，relay不可以...
待续...
这次写的很匆忙，假如有不正确的地方，请指正，谢谢😊。
