---
title: Array的reduce方法
date: 2017-07-28 22:21:28
tags: ['javascript']
categories: ['javascript']
description: reduce是一个很优雅的方法
---

### 初识

第一次听说Array的reduce方法是在面试的时候
这次看到大哥写的一个函数，里面用了reduce，哇，真的好优雅。因为代码的重复片段太多，我尝试去封装一个通用函数，没有成功。于是大哥出动了。
目的是这样的：我可能需要data.user.article的值，或者需要data.article的值，或者需要data.categories.article的值，于是封装一个方法每次取到article的值。
```javascript
// 封装一个函数reg，参数为belongto，传递的参数分别是：['user'] [] [categories]
// 函数内部的主要代码是
return belongto.reduce((p,c) => p['c'], data).article
```

### reduce文档

#### Syntax

```javascript
arr.reduce(callback[, initialValue])
```

#### 参数

callback有四个参数：分别是accumulator(它是callback上一次返回的值或者是initialValue，前提是initialValue存在)、currentValue(正在使用的值)、currentIndex(正在使用的值在数组中的索引)、array(这个是循环的数组)；
initialValue：用作第一次调用回调的初始值，如果不提供此参数，则第一次调用回调的初始值是数组的第一个元素。为了保证安全，最好提供这个值

tip: 当数组为空时，若提供initialValue，则最后的返回值是initialValue，否则，报错；
     当数组不为空时，若提供initialValue，则循环从index为0开始；否则循环从index为1开始，accumulator此时为index为0的值。

#### 返回值

回调函数的最后的返回值

### 用法

求数组元素的总和（告别for循环）
```javascript
var sum = [0, 1, 2, 3].reduce((a, b) => a + b, 0);
// sum is 6
```

连接数组
```javascript
[[0, 1], [2, 3], [4, 5]].reduce((a, b) => a.concat(b), []);
// result is [0, 1, 2, 3, 4, 5]
```

相同元素在数组中出现几次
```javascript
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce( (allNames, name) => {
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```



