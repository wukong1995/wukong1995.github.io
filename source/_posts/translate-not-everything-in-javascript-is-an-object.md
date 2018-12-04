---
date: 2018-11-29 19:25:26
title: 翻译：javascript中方的所有事物并不都是对象
tags: [翻译, javascript]
categories: [翻译]
---

[文章出处](http://blog.brew.com.hk/not-everything-in-javascript-is-an-object/)

关于javascript是面向对象编程（OOP）语言还是函数式语言，存在和很多混淆，其实，javascript都可以在两者中使用。
这会导致人们问：在JavaScript中所有事物是对象吗？函数是什么？
这篇post将会作出解释。

## 让我们从头开始吧
在JavaScript中有六种原始数据类型

* Booleans - `true` or `false`
* null
* undefined
* number - 双精度的64位的浮点数，JavaScript中没有整数
* string
* symbol (new in ES6)

除了这六种原始类型，ECMAScript标准也定义了一个`object`类型，它是一个键值对存储
```js
const object = {
  key: "value"
}
```

所以，简单来说，不是原始类型的是事物是一个`object`，并且包括`function`和`array`

> 所有的函数都是object

```js
// Primitive types
true instanceof Object; // false
null instanceof Object; // false
undefined instanceof Object; // false
0 instanceof Object; // false
'bar' instanceof Object; // false

// Non-primitive types
const foo = function () {}
foo instanceof Object; // true
```

## 原始类型（Primitive types）
原始类型没有挂载方法，所以你永远不会看见`undefined.toString()`。也是因为这个，原始类型是不可变的，因为没有附加方法能改变他。

你可以为原始类型重新分配给变量，但它将会是一个新的值，不是原来的值，也不是改变。
```js
const answer = 42
answer.foo = "bar";
answer.foo; // undefined
```

>原始类型是不可变的

此外，原始类型作为本身的值存储，不像object作为引用存储。这会影响相等性检查。
```js
"dog" === "dog"; // true
14 === 14; // true

{} === {}; // false
[] === []; // false
(function () {}) === (function () {}); // false
```

>原始类型作为值存储，object作为引用存储


## 函数（function）
函数是一种特殊类型的object，它具有一些特殊的属性，例如`call`和`contractor`
```js
const foo = function (baz) {};
foo.name; // "foo"
foo.length; // 1
```

就像普通的对象一样，你可以为对象增加新的属性。
```js
foo.bar = "baz";
foo.bar; // "baz"
```
这可以是函数成为一等公民（This makes functions a first-class citizen.）。因为他可以像任何其它对象一样当作参数传递给其他参数。

### 方法（method）
方法是一个对象属性，也是一个函数。
```js
const foo = {};
foo.bar = function () { console.log("baz"); };
foo.bar(); // "baz"
```

## 函数的构造函数
如果你有多个相同实现的对象，你可以将实现逻辑放在构造函数中，然后使用这个构造函数创建这些对象。

构造函数和普通函数没有什么区别，当函数在new关键字后使用，它被当作构造函数使用。

>任何函数都可以是构造函数
```js
const Foo = function () {};
const bar = new Foo();
bar; // {}
bar instanceof Foo; // true
bar instanceof Object; // true
```
一个构造函数会返回一个object，你可以在函数内部使用this增加新的属性。因此我们想要为多个对象的属性bar初始化为baz，我们可以创建一个新的构造函数Foo来封装这个逻辑
```js
const Foo = function () {
  this.bar = "baz";
};
const qux = new Foo();
qux; // { bar: "baz" }
qux instanceof Foo; // true
qux instanceof Object; // true
```

>你可以使用构造函数来创建一个新的对象

不带new关键字运行构造函数，就像Foo()，将会像普通的函数一样。函数内部的this将会指向运行上下文。因此如果我们在所有函数的外部调用Foo()，它实际上会修改window对象
```js
Foo(); // undefined
window.bar; // "baz"
```
相反，将普通的函数作为构造函数运行，你会得到一个新的空对象，正如你看到的那样。
```js
const pet = new String("dog");
```

## 包装对象(wrapper object)
由于函数如String，Number，Boolean，Function等而产生混淆，当使用new调用时，会为这些原始类型创建包装器对象。

String是一个全局函数，它在参数中传递时创建一个原始字符串;它会尝试将该参数转换为字符串。
```js
String(1337); // "1337"
String(true); // "true"
String(null); // "null"
String(undefined); // "undefined"
String(); // ""
String("dog") === "dog" // true
typeof String("dog"); // "string"
```

但您也可以使用String函数作为构造函数。
```js
const pet = new String("dog")
typeof pet; // "object"
pet === "dog"; // false
```

这将创建一个表示字符串“dog”的新对象（通常称为包装对象），具有以下属性：

>对象包装器通常也称为包装器对象。
>Object wrappers are often referred to as wrapper objects, too. Go figure.


### 自动装箱(Auto-Boxing)

有趣的是，原始字符串和对象的构造函数都是String函数。 更有趣的是你可以在原始字符串上调用.constructor，当我们已经了解了原始类型不能有方法时！

```js
const pet = new String("dog")
pet.constructor === String; // true
String("dog").constructor === String; // true
```

发生的事情是一个叫做自动装箱的过程。 当您尝试在某些基本类型上调用属性或方法时，JavaScript将首先将其转换为临时包装器对象，并访问其上的属性/方法，而不会影响原始对象。
```js
const foo = "bar";
foo.length; // 3
foo === "bar"; // true
```

在上面的示例中，要访问属性长度，JavaScript将autoboxed foo转换为包装器对象，访问包装器对象的length属性，然后将其丢弃。 这样做不会影响foo（foo仍然是一个原始字符串）。

这也解释了为什么当您尝试将属性分配给基本类型时JavaScript不会报错，因为赋值是在该临时包装器对象上完成的，而不是基本类型本身。

```js
const foo = 42;
foo.bar = "baz"; // Assignment done on temporary wrapper object
foo.bar; // undefined
```

如果你尝试使用没有包装器对象的原始类型，例如undefined或null，它会报错。
```js
const foo = null;
foo.bar = "baz"; // Uncaught TypeError: Cannot set property 'bar' of null
```

## 总结
1. 并非JavaScript中的所有内容都是对象
2. JavaScript中有6种原始类型
3. 所有不是原始类型的东西都是一个对象
4. 函数只是一种特殊类型的对象
5. 函数可用于创建新对象
6. 字符串，布尔值和数字可以表示为基本类型，但也可以表示为对象
7. 某些原始类型（字符串，数字，布尔值）似乎表现得像对象，因为JavaScript特色称为自动装箱。

PS:我在评论区发现了一张很好的图片
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1543729340/blog/acfab1be378bd89f96cab0b894e1ad7ee3cba7f98fa8ff19c96bd3e3027cb10e.png)
