---
date: 2018-09-13 21:47:18
title: Stateless component vs Pure component in react
tags: [react]
categories: [react]
description: stateless component vs pure component in react
---
### 介绍
#### 1. stateless component
**stateless component**声明为一个没有`state`的function

```js
const Article = (props) => {
  return (
    <div>
      // ...
    </div>
  )
}
```

react的文档中这样说到：
>These components must not retain internal state, do not have backing instances, and do not have the component lifecycle methods. They are pure functional transforms of their input, with zero boilerplate. However, you may still specify .propTypes and .defaultProps by setting them as properties on the function, just as you would set them on an ES6 class.

#### 2. pure component
**pure component**是最大的意义是优化React应用的性能。使用PureComponent可以大大的提高性能，因为它减少`render`的次数。

#### 3. 对比两者的性能
```js
class Welcome extends React.PureComponent {
  render() {
    return <h1>Welcome</h1>
  }
}

Hello = () => {
  return <h1>Hello</h1>;
}
```

以上例子是一个非常简单的`Welcome`(Pure Component)和`Hello`(Stateless Component)。**当你在父组件中使用它们，你会发现当父组件`re-render`时，`Hello`就会`re-render`，但是`Welcome`就不会**。

这是因为**PureComponent**改变了生命周期中的**shouldComponentUpdate**方法，并且添加了一些逻辑用来自动检查组件是否需要`re-render`。这允许`PureComponent`仅仅在检测到`state`或者`props`改变时，才会调用render方法。

### 使用
#### 1. 什么时候使用Pure Component
假如你创建一个字典的页面，在该页面中显示所有以A开头的单词的含义。这个时候，你可以写一个接收props为heading和meaning并返回视图的组件。假如你使用分页每次只显示10个单词，当滚动时，再去请求另外10个单词并且在父组件中更新`state`。在这种情况下，应该使用Pure Component，它将会避免render之前请求到的所有单词。

此外，在你要使用Component的生命周期函数时，你必须使用`Pure Components`，因为`stateless components`没有生命周期函数。

#### 2. 什么时候使用Stateless Component

假如你想要创建一个漂亮UI的lable用来评估个人资料的可信度，例如初学者，中级，高级。由于它是一个很小的组件，其重新渲染几乎没有任何区别，并为这种small case创建一个新的组件将是耗时的。如果你继续为很小很小的view创建组件，很快，你将会遇到更多的组件，在一个大型项目中，它们会变得很难管理。同时应该牢记Pure Component具有**shallowEqual**（浅比较）的特性。

#### 3. 结论
Pure Components会使性能大幅提升，因为它减少了应用中render的次数。这对于一份复杂的UI是个巨大的胜利，因此建议尽可能的使用。此外还有一些情况需要使用生命周期函数，在这种情况下，我们不能使用stateless components。

Stateless Components可以简单而快速的实现。这对于一个re-render代价小的非常小的UI视图是很好的。它们提供更清晰的代码和更少的文件来处理。它最好的使用场景应该是在父组件为Pure Component或者HOC中使用。


### 后记

项目中使用的`Apollo`，我尝试将stateless component改为pure component，但是组件还是会re-render。Apollo中数据是`immutiable`，并且每个item在list中都有key，但是不知道为什么还是会重新渲染，十分纳闷啊。我已经提了issue，期待有答复～如果找不到好的方法来解决，我只能为每个pure comoponent增加shouldComponentUpdate了...


[参考链接](https://medium.com/groww-engineering/stateless-component-vs-pure-component-d2af88a1200b)











