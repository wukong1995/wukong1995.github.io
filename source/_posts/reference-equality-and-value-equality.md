---
date: 2018-09-26 19:50:06
tags: [javascript, 翻译]
categories:
  - [前端, javascript]
  - [翻译]
title: 翻译：reference equality and value equality
description: 翻译：两种相等--值相等和引用相等
---

在编程语言世界中通常有两种类型的相等：
* reference equality
* value equality

#### 引入

思考下面的代码：
```javascript
var x = 12,
    y = 12;

var object = { x: 1, y: 2 };
var object2 = { x: 1, y: 2 };
```

正如你看到的，`object`和`object2`有相同的值。如果你看一下`object`和`object2`中的每个键以及它们对应的值，它们是相同的。在object和object2中，`x`的值是1，`y`的值是2。

但是当你想要在你的程序中检查object和object2是否相等，你会发现：这两个对象是不一样的。
```javascript
object == object2 // false
object === object2 // false
```

这是为什么呢？

#### reference equality
Objects是灰常复杂的数据结构。它们可以有很多key，这些key可以执行不同的值。这些值也可以是objects，所以**objects是可以嵌套的**。

如果你考虑事物的相等行，事实上你需要考虑两件事情：
* 一个事物是否意味着与另一个事物相同？
* 一个事物与其它东西完全一样？

如果我从现实世界中举一个例子：想象一下你有一个红色的跑车，你的邻居也有和你的车一样的车，相同的颜色，相同的发动机，相同的牌子。如果陌生人经过你的家，他们会说：嘿，这些人有相同的车。

但是，你邻居的车不是你的。你不会坐上邻居车，认为这是你的，对吧，或者至少你不应该。如果你撞坏了邻居的车，你可能会有一个不快乐的邻居，当然还有一些法律问题:)

区分你的车和邻居的车的最明显的地方是车牌。

事实上， JavaScript objects的内置了这种“牌照”，每个object的独特特性称为`reference`（引用）。
当你在js中比较object时，它们通过reference比较
```javascript
object == object2;
object === object2;
```

问题：object和object2是相等吗？实际上你分配变量的时候，也分配了reference。
你可以很容易的检查这个：
```javascript
object = object2;
object == object2; // true
object === object2; // true
```

也会有一些有趣的结果，代码如下：
```javascript
object = object2;
object.x = 12;

object.x; // 12
object2.x; // 12
```
在这个例子中，你做了一次将`x`这个key分配给object这个对象。object和object2指向**相同的引用**，所以只要一个变量更改，另一个变量也会更改。

JavaScript中的复杂的数据结构都遵循`reference equality`的原则，这包括**arrays**和objetcs，实际上通过typeof查看array的类型，得到的结果也是object。

**还有一些值像：numbers, strings, booleans or null / undefined，它们都遵循一种相等：value equality。**

#### value equality
像前面说的，reference equality回答的是object1和object2是否一样？这种检查很简单，它们非常有效。

说到Javascript中的**primitives**（~~这个我也不知道怎么翻译~~18.12.21更新：上面的primitives指的是Primitive types（基本类型）, 指的是上面的numbers等)，它们是不可以嵌套的。这种不能嵌套其它结构的结构称为**shallow data structures**（浅数据结构）。在这样的结构中，你可以以有效的方式执行value equality。


但是什么是value equality？思考下面的代码：
```javascript
var x = 12,
    y = 12;
```
在相等方面，数字是最简单的。你可以清楚的说x变量的值等于y变量的值（12在数学中等于12）但是如果这些变量遵循reference equality，它们是不一样的，因为它们是在不同的地方创建的。所以它们的引用是不同的。x的12可能是和y的12是不一样的。这真是太乱了。

幸运的是，members在JavaScript中是primitives，primitives在javascri中使用的是value equality进行比较。
所以看到这是不奇怪的：
```javascript
x == y; // true
x === y; // true
```
value equality回答的是这个疑问：一个事物是否意味着与另一个事物相同？

嵌套的数据结构是的相等更加难以比较。Objects有任意的key和value，它可以包含其他的objects。为了比较两个objects的相等，你可能需要下面的算法：
```javascript
/ Input: an object1 and object2
// Output: true if an object1 is equal in terms of values to object2

valueEqual(object1, object2):
  object1keys = <list of keys of object1>
  object2keys = <list of keys of object2>

  return false if length(object1keys) != length(object2keys)

  for each key in object1keys:
    return false if key not in object2keys
    return false if typeof(object1[key]) != typeof(object2[key])

    if object1[key] is an object:
      keyEqual = valueEqual(object1[key], object2[key])
      return false if keyEqual != false

    if object1[key] is a primitive:
      return false if object1[key] != object2[key]

  return true
```

呼，这里面有好多相等检查，这是一个递归算法。它在比较两个object是会执行上千次相等检查，这样的相等检查通常被称作 **deep equality checks**（深比较）。
更糟糕的是，这个算法是不会完成的。这是因为你可能创建了**循环的引用对象**。

PS：[参考链接](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)




