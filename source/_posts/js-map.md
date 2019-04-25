---
title: js中map那些事
date: 2019-04-25 19:09:31
tags: [javascript]
categories: [前端, javascript]
description: python中的hash_map有感
---

这是我看到leetcode中的第一题的python解答想到的map，求两数之和为target，只需要在数字num的基础上，查找数组中是否存在target-num即可，但是针对数组的查找，时间复杂度应该是O(n)， 但是判断map是否含有某个key，它的时间复杂度应该是O(1)。我看别人的解答是使用map来降低时间复杂度，这里先不考虑空间复杂度。

es6中有[map对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)，虽然object也是key-value的形式，但是object中的key只能是字符串，而map的key可以是任何类型。当你使用一个object作为key时，可能在不使用之后导致内存得不到释放，于是js也提供了WeakMap类型。

map提供了一些很方便的方法，供使用。我在项目中使用map不多，想了解一下map的应用场景。[参考链接](https://medium.com/front-end-weekly/es6-map-vs-object-what-and-when-b80621932373)

#### 文章中的观点主要有
>map主要用于快速搜索和查找数据中
>key的类型：在object中，key必须是简单类型-整数、字符串或symbols，但在map中，key可以是任何类型
>元素的顺序：map会保留元素对的原始顺序，在object中，不会保留。
>继承：map是object的一个实例，但是object不是map的实例
>map提供的语法比object中的语法更简单

#### map和object的应用场景
* object适用于只有简单类型的key，给key赋值为函数
* JSON直接支持object，在使用json的时候可以选择object
* 在有大量增加和删除key的场景中，map的性能会好一点
* 对需要保存key的顺序时，使用map
* map在大数据中表现的比object更优秀


#### 另外
删除object中的key，有两种方法
```js
delete obj.id
obj.id = undefined // for performance boost
```
第一种方法将从对象中完全删除该特定的属性，第二种方法实际上只是将key的映射值改成undefined，key还保留在对象中。不仅仅要考虑性能。

