---
title: 翻译：javascript中的对象，方括号和算法
tags: [翻译, javascript]
categories: [翻译]
date: 2019-01-09 18:28:15
---

[原文链接](https://medium.freecodecamp.org/javascript-objects-square-brackets-and-algorithms-e9a2916dc158)

JavaScript最强大的一个方面是能够动态引用对象的属性，在本文中，我们将了解他的工作原理以及它带给我们的好处。我们将快速浏览一下计算机科学中使用的一些数据结构。此外，我们将研究一种名为大O表示法的东西，用于描述算法的性能。

#### 对象简介
让我们从创建一个名为car的对象开始，每一个对象都有一个叫属性的东西。属性是属于一个对象的变量。car对象将有三个属性：make，model和color。

让我们看看它的样子：
```js
const car = {
  make: 'Ford',
  model: 'Fiesta',
  color: 'Red'
};
```

我们可以使用点表示法来引用对象的属性。例如，如果我们想要找出car的颜色，我们可以使用点表示法，就像这样：car.color

我们甚至可以使用console.log输出：

```js
console.log(car.color); //outputs: Red
```

引用属性的另一种方法是使用方括号表示法：

```js
console.log(car['color']); //outputs: Red
```

在上面的例子中，我们使用属性名称作为方括号内的字符串来获取与该属性名称对应的值。关于方括号表示法的优点在于我们还可以使用变量来动态获取属性。

也就是说，我们可以将其指定为变量中的字符串，而不是对特定属性名称进行硬编码：

```js
const propertyName = 'color';
const console.log(car[propertyName]); //outputs: Red
```

#### 使用带方括号表示法的动态查找
让我们看一个我们可以使用它的例子。假设我们经营一家餐馆，我们希望能够在菜单上获得商品的价格。这样做的一种方法是使用if / else语句。

让我们写一个接受名称并返回价格的函数：

```js
function getPrice(itemName){
  if(itemName === 'burger') {
    return 10;
  } else if(itemName === 'fries') {
    return 3;
  } else if(itemName === 'coleslaw') {
    return 4;
  } else if(itemName === 'coke') {
    return 2;
  } else if(itemName === 'beer') {
    return 5;
  }
}
```

虽然上面的方法有效，但是他不是理想的。在我们的code中有菜单的硬编码。现在如果我们的菜单改变了，我们不得不重写代码并且重新部署。此外，我们可以有一个很长的菜单，不得不写所有这些代码将是繁琐的。

更好的方法是分离我们的数据和逻辑。数据将包含我们的菜单，逻辑将从该菜单中查找价格。

我们可以将菜单表示为对象，其中属性名称（也称为键）对应于值。

在这种情况下，键将是项目名称，值将是项目价格：

```js
const menu = {
  burger: 10,
  fries: 3,
  coleslaw: 4,
  coke: 2,
  beer: 5
};
```

使用方括号表示法，我们可以创建一个接受两个参数的函数：菜单对象和一个菜名并且返回菜品的价格：

```js
const menu = {
  burger: 10,
  fries: 3,
  coleslaw: 4,
  coke: 2,
  beer: 5
};
function getPrice(itemName, menu){
  const itemPrice = menu[itemName];
  return itemPrice;
}
const priceOfBurger = getPrice('burger', menu);
console.log(priceOfBurger); // outputs: 10
```

这种方法的巧妙之处在于我们将数据与逻辑分开。在这个例子中，数据存在于我们的代码中，但它可以很容易地来自数据库或API。它不再与我们的查找逻辑紧密耦合，后者将菜品名称转换为菜品价格。

#### 数据结构和算法

计算机科学术语中的map是一种数据结构，它是键/值对的集合，其中每个键映射到相应的值。我们可以使用它来查找与特定键对应的值。 这就是我们在前面的例子中所做的。我们有一个菜品的名称，我们可以使用菜单对象查找该项目的相应价格。我们正在使用一个对象来实现一个map数据结构。

让我们看看为什么我们可能想要使用Map。 假设我们经营一家书店，并有一份书籍清单。 每本书都有一个名为国际标准书号（ISBN）的唯一标识符，这是一个13位数字。 我们将图书存储在一个数组中，希望能够使用ISBN查找它们。

一种方法是循环遍历数组，检查每本书的ISBN值，如果匹配则返回它：
```js
const books = [{
  isbn: '978-0099540946',
  author: 'Mikhail Bulgakov',
  title: 'Master and Margarita'
}, {
  isbn: '978-0596517748',
  author: 'Douglas Crockford',
  title: 'JavaScript: The Good Parts'
}, {
  isbn: '978-1593275846',
  author: 'Marijn Haverbeke',
  title: 'Eloquent JavaScript'
}];
function getBookByIsbn(isbn, books){
  for(let i = 0; i < books.length; i++){
    if(books[i].isbn === isbn) {
      return books[i];
    }
  }
}
const myBook = getBookByIsbn('978-1593275846', books);
```

这个例子很好用，因为我们只有三本书（这是一个小书店）。但是，如果我们是亚马逊，那么迭代数以百万计的书籍可能会非常慢并且计算成本也很高。

计算机科学中使用大O表示法来描述算法的性能。例如，如果n是我们集合中的书籍数量，那么在最坏的情况下使用迭代来查找书籍的成本（我们要查找的书是列表中的最后一本）将是O(n)。这意味着如果我们集合中的书籍数量增加一倍，使用迭代查找书籍的成本也会翻倍。

让我们看看如何通过使用不同的数据结构使我们的算法更有效。

如上所述，可以使用映射来查找与键对应的值。 我们可以使用对象将数据结构化为map(这里指对象)而不是数组。

键将是ISBN，值将是相应的书对象：
```js
const books = {
  '978-0099540946':{
    isbn: '978-0099540946',
    author: 'Mikhail Bulgakov',
    title: 'Master and Margarita'
  },
  '978-0596517748': {
    isbn: '978-0596517748',
    author: 'Douglas Crockford',
    title: 'JavaScript: The Good Parts'
  },
  '978-1593275846': {
    isbn: '978-1593275846',
    author: 'Marijn Haverbeke',
    title: 'Eloquent JavaScript'
  }
};
function getBookByIsbn(isbn, books){
  return books[isbn];
}
const myBook = getBookByIsbn('978-1593275846', books);
```

我们现在可以使用ISBN的简单地图查找来获取我们的价值，而不是使用迭代。我们不再需要检查每个对象的ISBN值。 我们使用key直接从地图获取值。

在性能方面，地图查找将提供相对于迭代的巨大改进。 这是因为地图查找在计算方面具有不变的成本。 这可以使用大O表示法写为O(1)。如果我们有三三百万册书籍没关系，我们可以通过使用ISBN键进行地图查找来快速获得我们想要的书。


#### 总结
* 我们可以使用点表示法和方括号表示法访问对象属性的值
* 我们学习了如何通过使用带方括号表示法的变量来动态查找属性值
* 我们还了解到，map数据结构将键映射到值。 我们可以使用键直接在我们使用对象实现的map中查找值。
* 我们初看了如何使用大O表示法描述算法性能。此外，我们还看到了如何通过将对象数组转换为map并使用直接查找而不是迭代来提高搜索性能。


ps: hardcode真的是很硬...
