---
title: javascript设计模式 - 总结
date: 2020-09-05 15:12:32
tags: [读书笔记]
categories: [读书笔记]
description: 反/构造器/单例/观察者/中介/外观模式
---

### 反模式(anti pattern)
针对一个问题的不良解决方案可能会导致糟糕的情况。在react blog中有一篇使用中的反模式，可以看一下[You Probably Don't Need Derived State](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

### Constructor pattern
在oop中，Constructor一种在内存已经分配该对象的情况下，用于初始化对象的特殊方法。但是js中，几乎所有东西都是对象，我们通常最感兴趣的是object constructors。
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1599292533/blog/httpatomoreillycomsourceoreillyimages1547797.png)

```js
function Car( model, year, miles ) {

  this.model = model;
  this.year = year;
  this.miles = miles;

  this.toString = function () {
    return this.model + " has done " + this.miles + " miles";
  };
}

// We can create new instances of the car
var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
```

* [The Constructor Pattern](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s01.html)
* [Difference between Constructor pattern and Prototype pattern](https://stackoverflow.com/questions/35057827/difference-between-constructor-pattern-and-prototype-pattern)

### Singleton pattern
它限制了类的实例化次数。Singleton模式，在实例不存在的情况下，可以通过一个方法创建；如果试过实例存在，它就会返回该对象的引用。

最近遇到的场景是，项目有一套自定义的配置模板，你需要使用js去读取它。但是在umi创建的项目中，发现这个文件会反复执行好几遍，于是可以使用单例，在config存在的情况下直接返回，不需要再进行读取文件的等等的操作

```js
const FOO_KEY = Symbol("foo")

global[FOO_KEY] = {
  foo: "bar"
}

// define the singleton API
// ------------------------

var singleton = {}

Object.defineProperty(singleton, "instance", {
  get: function(){
    return global[FOO_KEY]
  }
})

// ensure the API is never changed
// -------------------------------

Object.freeze(singleton)

// export the singleton API only
// -----------------------------

module.exports = singleton
```

* [The Singleton Pattern](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s04.html)
* [Creating A True Singleton In Node.js, With ES6 Symbols
](https://derickbailey.com/2016/03/09/creating-a-true-singleton-in-node-js-with-es6-symbols/)


### Observer pattern
一个对象维持一系列依赖它的对象，将有关任何变更自动通知给它们。
想想经常用的库，Rxjs中是不是有observer这个概念。

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1599292526/blog/httpatomoreillycomsourceoreillyimages1547801.png)

* [The Observer Pattern](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s05.html)
* [Observer vs Pub-Sub Pattern](https://medium.com/better-programming/observer-vs-pub-sub-pattern-50d3b27f838c)

### Mediator pattern
中介者是一种行为设计模式，它允许我们公开一个统一的接口，系统的不同部分可以通过该接口通信。

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1599292795/blog/httpatomoreillycomsourceoreillyimages1547805.png)

对于DOM事件冒泡和事件委托，如果所有的事件处理在document上，不是在单个的node节点上，当前document就充当了一个中介者的角色。

* [Is the use of the mediator pattern recommend?
](https://stackoverflow.com/questions/12534338/is-the-use-of-the-mediator-pattern-recommend)
* [The Mediator Pattern](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s06.html)

### Facade pattern
外观模式为更大的代码体提供了一个方便的更高层次的接口，能够隐藏其底层的真实复杂性。

最经常用jquery即是外观模式。

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1599297162/blog/httpatomoreillycomsourceoreillyimages1547811.png)

* [The Facade Pattern](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch09s09.html)










