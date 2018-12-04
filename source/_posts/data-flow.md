---
title: 单双向数据流中的父子组件如何进行交流
date: 2018-08-05 20:30:38
tags: [react, angularjs]
categories:
  - [react]
  - [前端, angularjs]
description: 单双向数据流中的父子组件如何进行交流
---

前话，我最开始接触的是Angular，再是Vue，工作之后就开始用React，之前习惯了双向数据绑定，刚开始写太顺手，但心中想着框架是相通的，于是抱着这个想法，继续下去，遗憾的是，我把不相通的地方当成了相通....最明显的例子应该就是父子组件因为数据而产生的通信

#### 双向数据流
对于angular来说，对于同一份数据，我既在父组件中可以进行更改，也可以在子组件中进行更改，通信功能其实很弱化。

#### 单向数据流
对于父组件中的数据，子组件如果有修改的需求，该怎么办？在查到的资料中，很多解决方案都提到了调用这个词，父组件调用子组件的方法，子组件调用父组件的方法。抱着这样的想法我会写出以下：
```javascript
const Parent = () => {
  return <MyChild {...someFunc}/>
};

class MyChild extends React.Component {
  delete(id) {
    // $.ajax
    this.props.someFunc();
  }
  render() {
    return (
      //
    )
  }
}
```
写了很久的上面的解决方案....
即是单向数据流，子组件调用了父组件的方法，我在调试的时候，假如看到数据的变化，我还需要去子组件里面查找，而且最典型的是对于一个数据的处理发生在了两个组件之间。这违背了单向数据流的思想。
于是，第二个解决方案来了：
```javascript
class Parent extends React.Component {
  delete() {
    // babbababa
  }
}

const MyChild = (props) => {
  return (
    <div onClick={props.delete}
  )
};
```
子组件变成一个纯组件，它如果想要对数据做什么，只是简单的告诉父组件，它本身没有任何的操作逻辑。所有的数据操作都发生在拥有数据的组件中。

结语：特性不相通，和特性有关的操作也不会相通的。


