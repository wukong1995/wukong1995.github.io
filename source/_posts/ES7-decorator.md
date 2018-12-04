---
title: ES7-decorator
date: 2017-08-13 12:09:44
tags: ['es7']
categories: [前端, javascript]
description: es7中的装饰器呀
---

### 装饰模式
对于装饰模式，现在还剩下鸡腿堡+香菜+辣椒的记忆。
这个是装饰模式的一个例子：有一个`汉堡`的抽象构件，`鸡腿堡`是具体构件，`香菜`和`辣椒`都是具体的装饰角色。现在我想计算鸡腿堡+香菜+辣椒的价格

#### 针对的问题
你想要动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。不改变接口的前提下，增强所考虑的类的性能。
何时使用：
1. 需要扩展一个类的功能，或给一个类增加附加责任。
2. 需要动态的给一个对象增加功能，这些功能可以再动态地撤销。
3. 需要增加一些基本功能的排列组合而产生的非常大量的功能，从而使继承变得不现实。

### es7中的装饰器
es7新增的decorator 属性，它借鉴自 Python，在 Python 里，decorator 实际上是一个 wrapper，它作用于一个目标函数，对这个目标函数做一些额外的操作，然后返回一个新的函数。
#### 装饰property
ES2016装饰器是一个返回函数的表达式，可以将target，name和property描述符作为参数。你可以通过在装饰器前加一个“@”字符来应用它，并将其放置在您想要装饰的顶部。可以为类或属性定义装饰器。

```js
class Cat {
    meow() {
        return `${this.name} say meow`;
    }
}

// 如果给meow方法加上可读属性
// 定义一个装饰器
function readonly (target, name, descriptor) {
    descriptor.writeable = false;
    return descriptor;
}

class Cat {
    @readonly
    meow() {
        return `${this.name} say meow`;
    }
}

// 此时你尝试修改meow，就会报错
//
// 在这里推荐一个module： core-decorators
```

#### 装饰class
在这种情况下，装饰器将使用目标target的构造函数。
```js
function hero(target) {
    target.isHero = true
}

@hero
class MyHero {}
console.log(MyHero.isHero); // true
```
还可以进一步扩展，为装饰功能提供参数。
```js
function hero(isHero) {
    return function(target) {
        target.isHero = isHero;
    }
}
// 你可以写成ES6的形式
// const hero = isHero => target => target.isHero = isHero;
@hero(false)
class MyHero {}
console.log(MyHero.isHero); // false
```

### 参考
[参考链接](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)

