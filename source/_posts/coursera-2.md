---
title: coursera第二周
date: 2017-08-26 16:29:03
tags: [编程语言]
categories: [编程语言]
description: 编程语言
---

### 说在前面的话
最初对待语言的看法：相比语言更重要的是由编程思想，语言只是一种工具。所以对于只停留在Syntax阶段，没有去深究语言的内在。
之前在学习C++的时候，老师也讲过一些内存方面...例如一段很简单的代码，在C++中会造成死循环...代码片段我还是没找到...
在学编译原理的时候，也学过词法分析、语法树等等...但是总是来说对于语言还是又一个模糊的概念
近期在coursera看一门课程，收获很大。刚开始看视频，因为自己的英语能力薄弱，刚开始看的时候，恨不得每句话都Google tanslate一遍，看多了就习惯了，因为大部分的术语你已经知道意思了，所以能知道大概的意思。Google翻译的意思，因为不知道术语所以翻译出来也很奇特。目前是以ML为例讲解的

### variable binding
变量的绑定主要包括两个环境：静态环境记录了变量的类型；动态环境记录了变量的值
```ML
val x = 34
(* static env : x : int *)
(* dynamic env : x -> 34 *)
```

### rules to expressions
它共有三个部分：语法检查、类型检查和评估规则
```ML
Syntax:
  if el then e2 else e3
  where if, then, and else are keywords and
  e1, e2, and e3 are subespressions

Type-checking:
  first el must have bool type
  e2 and e3 can hav any type(let`s call it t), but they
  must have the same type t
  the type of the entire expression is also t

Evalustion rules:
  first evalustion el to a value call it v1
  if it`s true, evaluate e2 and that result is the whole expression`s result
  else, evaluate e3 and that result is the whole expression`s result
```

### shadowing
当你重复声明相同的变量时，之前声明的值就会被覆盖，当你在REPL中看它的值的时候，就会变成hidden value，所以不建议重复声明变量
```ML
val x = 34
val x = 45    (* this is not assiginment statement *)
```

### 递归
ML语言没有for循环，所以在对于list类型的数据，会尝试使用递归去解决问题，但是小心哦，不恰当的使用递归，会使运行次数呈指数式增长。
下面是求list中的最大值，可以简单粗暴的理解成求数组中的最大值
```ML
fun bad_max(numbers: int list) =
  if null numbers
  then 0
  else if null tl numbers
  then hd numbers
  else if hd numbers > bad_max(tl numbers)
  then hd numbers
  else bad_max(tl numbers)
```
上面代码逻辑清晰，通俗易懂，但是使用的时候，假如数组中的数是从大到小排列时，程序运行速度很快；相反，数组中的数若是从小到大排列，当数组是[1,...30]时，你就会发现有延迟...讲师用了一张图给你讲解
![](http://res.cloudinary.com/dwudaridr/image/upload/v1503738949/WX20170825-142135_2x_oq3vsb.png)

优化的方法是：你可以将`bad_max(tl numbers)`的值赋予一个变量，这样，每次程序运行时`bad_max(tl numbers)`只会执行一遍。这里使用了`let...in...end`，在这个课程中，作者也讲解了作用域的问题，例如子作用域的值回覆盖父作用域的值...

### 关键字
每门语言都会提供关键字来提高代码的可读性，在适当的地方记得使用

### 调试错误
调试错误时，一定要有耐心...
