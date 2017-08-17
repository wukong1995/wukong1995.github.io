---
title: immutable-data
date: 2018-09-25 19:02:43
articleTitle: immutable data
tags: ['react']
categories: ['react']
---

项目中的apollo主要是用的分页这个功能，但是我在check代码的时候，发现了每次我向列表中push了一个数据，整个列表是全部re-render!!!假如每个是十条数据，等我滑到第十页的时候，100个item是全部re-render的...我去查apollo fetch updata 没有找到结果，我向react-apollo提了issue，感谢G友captDaylight给我的回复。

>When you update an item in a list with immutablility helper its returning a brand new array, so it'll have a new reference. So that list in React's eyes is completely new, even if structurally most of the items in the array are the same. Thus each item will be re-rendered.

开始，我的重点是放在immutablility-helper，但是查了好久也没找到why

这个时候，我查了项目中使用state的列表，发现它只会re-render新增的数据，于是我使用redux来渲染list，发现它也是全部re-render所有item。state和redux的差异在哪？redux中使用的是immutable data，于是我去查文档。

##### 1. What are the benefits of immutability
immutability可以为你的应用程序带来更高的性能，并且可以简化变成和调试。在整个应用程序中，从不会更改的数据比可以随意更改的数据更容易理解。

特别的，Web应用程序环境中的不变性可以使复杂的变更检测变得更简单和廉价，确保计算成本高昂的DOM更新过程只发生在绝对必要的时候（这是React相对于其他库性能改进的基石）。

##### 2. Why is immutability required by Redux?
* Redux和React-Redux都使用浅等式检查。尤其是：
  + Redux的combineReducers实用程序浅层检查由它调用的reducer引起的引用变化。
  + React-Redux的connect方法生成的组件浅层地检查对根状态的引用更改，以及来自mapStateToProps函数的返回值，以查看包装的组件是否实际需要re-render
* immutable data的管理最终使数据更安全
* Time-travel debugging要求reducers是没有副作用的纯函数，因此你可以在不同的状态之间正确跳转。

##### 3. Why does Redux’s use of shallow equality checking require immutability?
如果要正确的更新任何connected组件，Redux使用 shallow equality checking（浅检查），要了解原因，我们需要了解 shallow equality checking和deep equality checking（深检查）之前的区别。
两者的区别是：shallow equality checking只是简单的检查两个不同的变量是否是相同的引用；相反，deep equality checking必须检查两个对象属性的每个值。

##### 4. How does Redux use shallow equality checking？
Redux在combineReducers函数中使用浅等式检查来返回根状态对象的新变异副本，或者，如果没有进行任何突变，则返回当前根状态对象。

##### 5.How does React-Redux use shallow equality checking?
当根状态对象的引用改变时，对于有mapStateToProps的组件，检查mapStateToProps的返回值，如果返回值不变，则组件不会re-render。

对于下面这个组件，如果state.todos和getVisibleTodos返回的值不变时，这个组件不会re-render
```js
function mapStateToProps(state) {
  return {
    todos: state.todos, // prop value
    visibleTodos: getVisibleTodos(state) // selector
  }
}
​
export default connect(mapStateToProps)(TodoApp)
```

对于下面这个组件，始终会re-render，因为todos的值始终是新对象，无论它的值是否更改
```js
// AVOID - will always cause a re-render
function mapStateToProps(state) {
  return {
    // todos always references a newly-created object
    todos: {
      all: state.todos,
      visibleTodos: getVisibleTodos(state)
    }
  }
}
​
export default connect(mapStateToProps)(TodoApp)
```

##### 6. 如何避免不必要的render？
和shouldComponentUpdate搭配使用，PS:shouldComponentUpdate返回的值是建议性的，所以在组件中你返回false，组件有可能re-render。
