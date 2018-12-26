---
date: 2018-12-24 18:24:29
title: 翻译：理解javascript中的作用域
tags: [翻译, javascript]
categories:
  - [翻译]
  - [javascript]
---

[原文链接](https://scotch.io/tutorials/understanding-scope-in-javascript)


### #简介
`JavaScript`有一个特性叫做作用域。尽管作用域的概念对于许多初学者是不容易理解的，我会尽力在最简单的范围内解释。理解作用域会是你的代码更加清楚，减少错误，帮助你使用它制作强大的设计模式。


### #什么是作用域
作用域是运行时代码中某些特定部分中变量，函数和对象的可访问性。换句话说，作用域确定了代码中的变量和其他资源的可见性。

### #作用域为何存在--最小访问性原则
因此，限制变量的可见性的重点是什么，而不是所有的代码不是随处可见的。一个优点是作用域为你的代码提供了一定级别的安全性，计算机安全的一个常见的原则是用户应该一次只能访问他们需要的东西。

想想电脑的管理员，由于他们对公司的电脑有很多控制权，向他们的账户授予全部权限是没问题的。假设你有一个含有三个管理员的公司，他们都可以访问系统，一切都很顺利。但是突然，发生了一件坏事，其中的一个系统感染了病毒。现在你不知道到底是谁的错误导致的。你意识到你应该给他们基本的用户账户，只有在需要的时候才赋予完全访问的特权。这会帮助你追踪变化，记录谁做了什么。这叫做最小访问性原则。看起来很直观？这个原则也适用于程序语言的设计。它在大多数编程语言中称作作用域，包括我们接下来要研究的JavaScript。

随着你的编程之旅，你会意识到代码的作用域有助于提高效率，追踪bug并且减少bug。作用域也解决了在不同作用域中相同变量名的命名问题。切记不要吧作用域和上下文弄混淆了，它们是不同的特性。

### #JavaScript中的作用域
在JavaScript中有两种类型的作用域
* 全局作用域
* 局部作用域

函数内部的变量是在局部作用域，外部的是在全局作用域。每一个函数在调用的时候会创建一个新的作用域。

### #全局作用域
在文档中开始写JavaScript时，你已经在全局作用域中了。整个JavaScript文件中只有一个全局作用域，如果变量位于函数的外面，那么它是在全局作用域中。

```js
// the scope is by default global
var name = 'Hammad';
```

位于全局作用域的变量可以在其他作用域被访问和修改。
```js
var name = 'Hammad';

console.log(name); // logs 'Hammad'

function logName() {
    console.log(name); // 'name' is accessible here and everywhere else
}

logName(); // logs 'Hammad'
```

### #局部作用域
定义在函数内部的变量是在局部作用域中，每一次调用函数，它们会有不同的作用域。这意味着相同名字的变量可以在不同的函数中使用。这是因为这些变量绑定在它们各自的函数中，每一个有不同的作用域，并且在其他的函数中无法访问。
```js
// Global Scope
function someFunction() {
    // Local Scope #1
    function someOtherFunction() {
        // Local Scope #2
    }
}

// Global Scope
function anotherFunction() {
    // Local Scope #3
}
// Global Scope
```

### #块语句
块语句类似`if`和`switch`条件或者`for`和`while`循环中，不像函数那样创建新的作用域。定义在块语句中的变量将保留在它们已经存在的作用域中。
```js
if (true) {
    // this 'if' conditional block doesn't create a new scope
    var name = 'Hammad'; // name is still in the global scope
}

console.log(name); // logs 'Hammad'
```

ECMAScript 6中采用let和const关键字，这些关键词可以代替var关键字。
```js
var name = 'Hammad';

let likes = 'Coding';
const skills = 'Javascript and PHP';
```

与`var`关键字相反，`let`和`const`关键字支持在块语句中声明局部作用域。
```js
if (true) {
    // this 'if' conditional block doesn't create a scope

    // name is in the global scope because of the 'var' keyword
    var name = 'Hammad';
    // likes is in the local scope because of the 'let' keyword
    let likes = 'Coding';
    // skills is in the local scope because of the 'const' keyword
    const skills = 'JavaScript and PHP';
}

console.log(name); // logs 'Hammad'
console.log(likes); // Uncaught ReferenceError: likes is not defined
console.log(skills); // Uncaught ReferenceError: skills is not defined
```

>只要您的应用程序存在，全局作用域就会存在。只要调用和执行函数，本地作用域就会存在。

### #上下文
很多开发者经常混淆作用域和上下文，认为它们指的是相同的概念。但这种情况并非如此。作用域是我们上面讨论的，上下文用来在代码的某些特定部分引用`this`的值。作用域是指变量的可见性，而上下文是指在同一范围内的`this`的值。我们也可以使用函数方法更改上下文，我们将在后面讨论。在全局作用域中上下文始终是`Window`对象。
```js
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
console.log(this);

function logFunction() {
    console.log(this);
}
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
// because logFunction() is not a property of an object
logFunction();
```

如果作用域在一个对象的方法中，则上下文是该方法所属的对象。
```js
class User {
    logName() {
        console.log(this);
    }
}

(new User).logName(); // logs User {}
```

>`(new User).logName()`是一种将对象存储在变量中然后在其上调用`logName`函数的简短办法。在这里，你不需要创建新的对象。

你会注意到如果使用`new`调用你的函数，上下文的值表现是不一样的。上下文将会被设置为调用函数的实例。考虑上面的一个示例，使用`new`关键字调用该函数。
```js
function logFunction() {
    console.log(this);
}

new logFunction(); // logs logFunction {}
```

>在严格模式下调用函数时，上下文默认为`undefined`。

### #执行上下文
要消除我们上面学的内容的混淆，执行上下文中的上下文指的是作用域而不是上下文。这是一个奇怪的命名约定，但是由于JavaScript的规范，我们与之相关。

JavaScript是一个单线程的语言，所以它一次只能执行一个任务。其余的任务在执行上下文中排队。正如我之前告诉你的，当JavaScript解释器开始执行你的代码时，默认情况下，上下文（作用域）设置成全局。此全局上下文附加到您的执行上下文，该上下文实际上是启动执行上下文的第一个上下文。

之后，每个函数调用都会将其上下文附加到执行上下文。当在该函数内部或其他地方调用另一个函数时，会发生同样的事情。

>每个函数都创建自己的执行上下文

一旦浏览器完成该上下文中的代码，那么该上下文将从执行上下文中弹出，并且执行上下文中的当前上下文的状态将被传送到父上下文。 浏览器总是执行位于执行堆栈顶部的执行上下文（实际上是代码中最内层的范围）。

>只能有一个全局上下文，但有任意数量的函数上下文。

执行上下文有两个创建阶段和代码执行阶段。

#### 创建阶段
当调用函数但其代码尚未执行时，存在创建阶段的第一个阶段。在创建阶段发生的三件主要事情是：
* 创建可变对象
* 创建作用域链
* 设置上下文的值（this）

##### 可变对象
可变对象（也称为激活对象）包含在执行上下文的特定分支中定义的所有变量，函数和其他声明。 调用函数时，解释器会扫描所有资源，包括函数参数，变量和其他声明。 当打包到单个对象中时，所有内容都将成为可变对象。
```js
'variableObject': {
  // contains function arguments, inner variable and function declarations
}
```

##### 作用域链
在创建阶段的运行上下文中，作用域链在可变对象后被创建。作用域链本身包含变量对象。作用域链被用来解决变量。当被要求解析变量时，JavaScript始终从代码嵌套的最内层开始，并一直跳回到父作用域，直到它找到正在寻找的变量或任何其他资源。作用域链可以简单地定义为包含其自己的执行上下文的可变对象的对象，以及它父对象的所有其他执行上下文，该对象拥有一堆其他对象。
```js
'scopeChain': {
    // contains its own variable object and other variable objects of the parent execution contexts
}
```

##### 执行上下文对象
执行上下文对象可以表示为这样的抽象对象：
```js
executionContextObject = {
  'scopeChain': {}, // contains its own variableObject and other variableObject of the parent execution contexts
  'variableObject': {}, // contains function arguments, inner variable and function declarations
  'this': valueOfThis
}
```

#### 代码执行阶段
在执行上下文的第二阶段是代码执行阶段，其他的值被分配，代码最终运行。

### #语法作用域
语法作用域意味着嵌套在函数组中，内部的函数可以访问其父作用域的变量和其他资源。这意味着子函数在语法上绑定了其父函数的执行上下文。语法作用域有时也被称为静态作用域。
```js
function grandfather() {
    var name = 'Hammad';
    // likes is not accessible here
    function parent() {
        // name is accessible here
        // likes is not accessible here
        function child() {
            // Innermost level of the scope chain
            // name is also accessible here
            var likes = 'Coding';
        }
    }
}
```

你会注意到关于语法作用域的事情是它向前工作，这意味着`name`可以通过其子项的执行上下文来访问。但是他不会像父母一样向后工作，这意味着变量`likes`不能被它的父函数访问。这也告诉我们在不同执行上下文中具有相同名称的变量从执行堆栈的顶部到底部优先获得。一个变量和其他变量具有相同的名称，在最里面的函数（执行堆栈的最顶部的上下文）具有最高的优先权。

### #闭包
闭包的概念和我们上面学习的语法作用域密切相关。当内部函数尝试访问其外部函数的作用域链时创建`Closure`，这意味着语法作用域之外的变量。闭包包含自己的范围链，父母的范围链和全局范围。

>闭包不仅可以访问外部函数中定义的变量，还可以访问外部函数的参数。

即使在函数返回后，闭包也可以访问其外部函数的变量。这允许返回的函数维护对外部函数的所有资源的访问。

当您从函数返回内部函数时，这时候您尝试调用外部函数时，将不会调用返回的函数。你必须首先将外部函数的调用保存在单独的变量中，然后将该变量作为函数调用。思考这个例子：
```js
function greet() {
    name = 'Hammad';
    return function () {
        console.log('Hi ' + name);
    }
}

greet(); // nothing happens, no errors

// the returned function from greet() gets saved in greetLetter
greetLetter = greet();

// calling greetLetter calls the returned function from the greet() function
greetLetter(); // logs 'Hi Hammad'
```

这里要注意的关键点是函数`greetLetter`在`greet`函数返回的情况下任然可以访问变量`name`。在没有变量赋值的情况下从`greet`函数调用返回函数的一种方法是使用括号两次，如下所示：
```js
function greet() {
    name = 'Hammad';
    return function () {
        console.log('Hi ' + name);
    }
}

greet()(); // logs 'Hi Hammad'
```
>PS：我觉得上面的例子没有很好的反映出来闭包的概念，让我看来它只是一个高阶函数的代表。我第一次接触闭包是我之前在写一个手风琴的demo时，为每一个li标签添加事件，但是写出来的结果是只能为最后一个li标签添加事件，其他的列表没有添加上！what？我是来了一个for循环加上的呀...这时候我开始寻找问题的原因，进而了解到闭包这个概念。

### #public作用域和private作用域
在很多其他的编程语言中，你可以使用`private`、`public`、`private`和`protected`设置类方法和类属性的可见性。使用PHP语言思考这个例子：
```php
// Public Scope
public $property;
public function method() {
  // ...
}

// Private Sccpe
private $property;
private function method() {
  // ...
}

// Protected Scope
protected $property;
protected function method() {
  // ...
}
```

封装来自public作用域的函数可以使他们免受易攻击。但是在JavaScript中，共有和私有的概念都没有。然而，我们可以使用闭包来模拟这个特性。为了使所有内容与全局分离，我们必须首先将函数封装在如下的函数中：
```js
(function () {
  // private scope
})();
```

函数的尾部告诉解释器无需调用立即就可以执行它。我们向其中增加函数和变量，在外部是不可以访问的。但是，如果我们想在外部访问它们，意味着我们希望一部分是public一部分是private？我们可以使用的另一种闭包叫做模块模式，这允许我们使用对象中的public和private作用域来界定我们的函数。

#### 模块模式
模块模式看起来像这样：
```js
var Module = (function() {
    function privateMethod() {
        // do something
    }

    return {
        publicMethod: function() {
            // can call privateMethod();
        }
    };
})();
```
模块的返回声明中包括我们的public函数，private函数并不会返回。不反悔的函数是在模块命名空间外不能访问。但是我们的共有方法是可以访问我们的私有函数，这些函数一般是辅助函数，例如ajax调用和一切其他的。
```js
Module.publicMethod(); // works
Module.privateMethod(); // Uncaught ReferenceError: privateMethod is not defined
```

一种惯例是私有函数的命名是以下划线开头，并返回包含我们的公共函数的匿名对象。这使得它们易于在长对象中进行管理。这就是它的样子：
```js
var Module = (function () {
    function _privateMethod() {
        // do something
    }
    function publicMethod() {
        // do something
    }
    return {
        publicMethod: publicMethod,
    }
})();
```

#### 立即执行函数（IIFE）
另一种闭包的类型是立即执行函数。这是在`window`的上下文中自调用的匿名函数，这意味着`this`的值是`window`。这暴露了一个与之交互的全局接口。它看起来是这样：
```js
(function(window) {
    // do anything
})(this);
```

### #使用.call(), .apply() 和 .bind()改变上下文
`call`和`apply`函数用于在调用函数时更改上下文，这为您提供了令人难以置信的编程能力（以及统治世界的一些终极能力）。要使用`call`或`apply`函数，只需要在函数上调用它，而不是使用一对括号调用函数，并将上下文作为第一个参数传递。函数自己的参数可以在上下文之后传递。
```js
function hello() {
    // do something...
}

hello(); // the way you usually call it
hello.call(context); // here you can pass the context(value of this) as the first argument
hello.apply(context); // here you can pass the context(value of this) as the first argument
```

`.call()`和`.apply()`之间的区别在于，在`call`中，您将其余参数作为以逗号分隔的列表传递，而`apply`允许您传递数组中的参数。
```js
function introduce(name, interest) {
    console.log('Hi! I\'m '+ name +' and I like '+ interest +'.');
    console.log('The value of this is '+ this +'.')
}

introduce('Hammad', 'Coding'); // the way you usually call it
introduce.call(window, 'Batman', 'to save Gotham'); // pass the arguments one by one after the contextt
introduce.apply('Hi', ['Bruce Wayne', 'businesses']); // pass the arguments in an array after the context

// Output:
// Hi! I'm Hammad and I like Coding.
// The value of this is [object Window].
// Hi! I'm Batman and I like to save Gotham.
// The value of this is [object Window].
// Hi! I'm Bruce Wayne and I like businesses.
// The value of this is Hi.
```

>`call`性能略高于`apply`。

以下示例获取文档中的项目列表，并将它们逐个打印到控制台:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Things to learn</title>
</head>
<body>
    <h1>Things to Learn to Rule the World</h1>
    <ul>
        <li>Learn PHP</li>
        <li>Learn Laravel</li>
        <li>Learn JavaScript</li>
        <li>Learn VueJS</li>
        <li>Learn CLI</li>
        <li>Learn Git</li>
        <li>Learn Astral Projection</li>
    </ul>
    <script>
        // Saves a NodeList of all list items on the page in listItems
        var listItems = document.querySelectorAll('ul li');
        // Loops through each of the Node in the listItems NodeList and logs its content
        for (var i = 0; i < listItems.length; i++) {
          (function () {
            console.log(this.innerHTML);
          }).call(listItems[i]);
        }

        // Output logs:
        // Learn PHP
        // Learn Laravel
        // Learn JavaScript
        // Learn VueJS
        // Learn CLI
        // Learn Git
        // Learn Astral Projection
    </script>
</body>
</html>
```

HTML仅包含无序的项列表。然后JavaScript从`DOM`中选择所有的列表。循环列表。在循环内部，我们将列表项的内容记录到控制台。

此日志语句包含在括在括号中的函数中，在该函数中调用调用函数。相应的列表项将传递给调用函数，以便控制台语句中的关键字记录正确对象的`innerHTML`。

对象可以有这些方法，同样函数对象也可以有这些方法。事实上，JavaScript函数带有四个内置方法，它们是：

* Function.prototype.apply()
* Function.prototype.bind() (Introduced in ECMAScript 5 (ES5))
* Function.prototype.call()
* Function.prototype.toString()


>Function.prototype.toString()返回函数源代码的字符串表示形式。

到目前为止，我们已经讨论过`.call()`,`.apply()`和`toString()`。 与`call`和`apply`不同，`bind`本身不调用该函数，它只能在调用函数之前用于绑定上下文和其他参数的值。 在上面的一个例子中使用`bind`：
```js
(function introduce(name, interest) {
    console.log('Hi! I\'m '+ name +' and I like '+ interest +'.');
    console.log('The value of this is '+ this +'.')
}).bind(window, 'Hammad', 'Cosmology')();

// logs:
// Hi! I'm Hammad and I like Cosmology.
// The value of this is [object Window].
```

`bind`就像`call`函数一样，它允许你一个接一个地用逗号分隔其余的参数，而不是像`apply`一样，在数组中传递参数。


### #总结
这些概念对JavaScript来说是激进的，如果您想要处理更高级的话题，这一点很重要。 我希望你能更好地理解JavaScript Scope及其周围的事情。如果有什么疑问，请随时在下面的评论中询问。

扩展您的代码，直到那时，快乐编码！
>Scope up your code and till then, Happy Coding!


### PS
终于理解编辑的累了，这么长的文章！！！这是分好几次做的...




