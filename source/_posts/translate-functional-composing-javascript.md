---
title: 翻译：JavaScript中的组合函数
date: 2019-01-17 18:35:09
tags: [翻译, javascript]
categories: [翻译]
description: 为什么赶时髦的人组合一切
---

[原文链接](http://busypeoples.github.io/post/functional-composing-javascript/)这篇文章很有趣。

#### 简介
lodash和underscore无处不在，但仍然有一种超级高效的方法，实际上只有那些赶时髦的人使用：组合。

我们将研究组合，并且深入了解为什么这种方法会是你的代码更加具有可读性，更易于维护和更加优雅。

#### 基础
我们将会使用`lodash`的一些函数，只是因为：
* 我们不想编写自己的简单的实现，因为它们会分散我们关注的内容
* `lodash`被广泛使用，可以很容易的被`underscore`或其他的库或者自己的实现替换

在我们深入研究一些基本的例子之前，让我们回顾一下“组合”实际做了什么，以及如果需要，我们如何实现我们自己的组合函数。
```js
var compose = function(f, g) {
    return function(x) {
        return f(g(x));
    };
};
```

这是最基本的实现。仔细看看上面的函数，你将会注意到传入的函数实际上是从右向左调用的，意思是将右侧函数的结果传递给它左侧的函数。

现在仔细看看这段代码：
```js
function reverseAndUpper(str) {
  var reversed = reverse(str);
  return upperCase(reversed);
}
```
*reverseAndUpper*函数首先反转给定的字符串，然后变大写。我们可以借助基本的组合函数重写以下代码：

```js
var reverseAndUpper = compose(upperCase, reverse);
```

现在我们可以使用`reverseAndUpper`：
```js
reverseAndUpper('test'); // TSET
```

这相当于写成：
```js
function reverseAndUpper(str) {
  return upperCase(reverse(str));
}
```
仅仅更加优雅、可维护和可重复使用。

快速组合函数的能力和创建数据管道的能力可以通过多种方式加以利用，从而可以在很长的管道中进行数据转换。想象一下传递一个集合，映射集合，然后在管道的末尾但会一个最大值或者将给定字符串转换成布尔值。组合是我们能够轻松的连接多个函数用来构建更复杂的功能。

让我们实现一个可以处理任意数量的函数以及任意数量的参数的非常灵活的组合函数，之前的组合函数只适用于两个函数或者只接受第一个参数传入，我们可以重写组合函数如下：
```js
var compose = function() {
  var funcs = Array.protoype.slice.call(arguments);

  return funcs.reduce(function(f,g) {
    return function() {
      return f(g.apply(this, arguments));
    };
  });
};
```

这个组合函数能够写出像这样的代码：
```js
var doSometing = compose(upperCase, reverse, doSomethingInitial);

doSomething('foo', 'bar');
```

存在大量的可以微我们实现组合的库。我们的组合函数应该只有助于理解`underscores`或`scoreunders`组成函数中真正发生的事情，显然具体实现在库之间有所不同。它们大部分仍然在做同样的事情：使它们各自的组成函数更加通用。现在我们已经知道什么叫组合了，让我们使用`lodash`中的 `_.compose`函数继续下面的例子。


### 栗子
让我们从一个很基础的例子开始：
```js
function notEmpty(str) {
    return ! _.isEmpty(str);
}
```

函数`notEmpty`简单的否定了`_.isEmpty`的结果。
我们可以使用lodash中的`_.compose`写一个`not`函数来做同样的事情。
```js
function not(x) { return !x; }

var notEmpty = _.compose(not, _.isEmpty);
```

现在我们可以在给定的任意参数调用`notEmpty`
```js
notEmpty('foo'); // true
notEmpty(''); // false
notEmpty(); // false
notEmpty(null); // false
```
第一个例子非常简单，下一个会更高级：

`findMaxForCollection`会返回由`id`和`value`属性组成的给定对象集合的最大的`id`。
```js
function findMaxForCollection(data) {
    var items = _.pluck(data, 'val');
    return Math.max.apply(null, items);
}

var data = [{id: 1, val: 5}, {id: 2, val: 6}, {id: 3, val: 2}];

findMaxForCollection(data);
```

上面的例子可以使用组合函数重写。
```js
var findMaxForCollection = _.compose(function(xs) { return Math.max.apply(null, xs); }, _.pluck);

var data = [{id: 1, val: 5}, {id: 2, val: 6}, {id: 3, val: 2}];

findMaxForCollection(data, 'val'); // 6
```
我们可以在这重构很多。

`_.pluck`期待集合作为第一个参数，回调函数作为第二个参数。如果我们想要部分应用`_.pluck`怎么办？这种情况下，我们可以使用柯里化来反转参数。
```js
function pluck(key) {
    return function(collection) {
        return _.pluck(collection, key);
    }
}
```

我们的`findMaxForCollection`仍然需要更加精致，我们可以创建我们的max函数。
```js
function max(xs) {
    return Math.max.apply(null, xs);
}
```

它可以使我们重写组合函数来变得更加优雅：
```js
var findMaxForCollection = _.compose(max, pluck('val'));

findMaxForCollection(data);
```

通过编写我们自己的`pluck`函数，我们可以用`val`部分地应用`pluck`。现在你可以明显的争辩，当`lodash`已经拥有更方便的`_.pluck`函数，为什么要编写自己的`pluck`方法？原因是`_.pluck`期望集合作为第一个参数，这不是我们想要的。通过反转参数，我们可以将部分应用key，只需要在返回的函数上调用数据。

我们还可以进一步重写pluck函数。`lodash`带来了一个更简单的方法：`_.curry`，这使我们能够编写如下的pluck函数：
```js
function plucked(key, collection) {
    return _.pluck(collection, key);
}

var pluck = _.curry(plucked);
```

我们只是想包裹原始的pluck函数，这样我们就可以翻转参数了。现在pluck仍简单的返回一个函数，这么长，直到所有的参数都被提供。让我们看一下最终的代码：
```js
function max(xs) {
    return Math.max.apply(null, xs);
}

function plucked(key, collection) {
    return _.pluck(collection, key);
}

var pluck = _.curry(plucked);

var findMaxForCollection = _.compose(max, pluck('val'));

var data = [{id: 1, val: 5}, {id: 2, val: 6}, {id: 3, val: 2}];

findMaxForCollection(data); // 6
```

`findMaxForCollection`可以很容易的从右向左阅读，这意味着集合先返回val的属性，然后获的所有给定值的最大值。
```js
var findMaxForCollection = _.compose(max, pluck('val'));
```

这使代码更具有可维护、可重用，并且更加优雅。我们将看一下最后的例子，以突出组合函数的优雅。

让我们扩展前一个示例的数据，然后添加名为active的属性，现在的数据如下：
```js
var data = [{id: 1, val: 5, active: true},
            {id: 2, val: 6, active: false },
            {id: 3, val: 2, active: true }];
```

我们有一个名为的`getMaxIdForActiveItems(data)`函数，它接收一组对象，过滤所有的active项，并从返回的过滤项中返回最大的id。
```js
function getMaxIdForActiveItems(data) {
    var filtered = _.filter(data, function(item) {
        return item.active === true;
    });

    var items = _.pluck(filtered, 'val');
    return Math.max.apply(null, items);
}
```

如果我们可以将上面的代码转换成更优雅的内容怎么办？我们已经有了`max`和`pluck`函数，所以我们现在要做的是添加过滤：
```js
var getMaxIdForActiveItems = _.compose(max, pluck('val'), _.filter);

getMaxIdForActiveItems(data, function(item) {return item.active === true; }); // 5
```

`_.filter`有和`_.pluck`一样的问题，这意味着我们不能将集合作为第一个参数来部分应用。我们可以通过包裹原生的`filter`实现，在`filter`上翻转参数。
```js
function filter(fn) {
    return function(arr) {
        return arr.filter(fn);
    };
}
```

另一个改进是添加一个`isActive`函数，它只需要一个项目并检查active标志是否设置为true。
```js
function isActive(item) {
    return item.active === true;
}
```

我们可以在filter上部分应用`isActive`，使我们只能使用集合调用getMaxIdForActiveItems。
```js
var getMaxIdForActiveItems = _.compose(max, pluck('val'), filter(isActive));
```

现在我们需要传递的是数据：
```js
getMaxIdForActiveItems(data); // 5
```

这也使我们能够轻松编写一个函数，返回任何非active的最大id：
```js
var isNotActive = _.compose(not, isActive);

var getMaxIdForNonActiveItems = _.compose(max, pluck('val'), filter(isNotActive));
```

#### 总结
组合函数可以很有趣，如前面的例子中所示，如果正确应用，可以产生更优雅和可重复使用的代码。


PS: 赞赞赞👍，一个简单的实现组合函数如下：
```js
const doubleValue = (x) => x * 2;
const multiplyByFour = x => x + 4

const compose = (...funcs) => (value) => {
  return funcs.reduceRight((acc, func) => func(acc), value)
}

// (((f, g) => x) === f(g(x))
console.log(compose(doubleValue, multiplyByFour)(2)) // 12
```
