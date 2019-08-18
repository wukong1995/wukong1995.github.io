---
title: 在高阶组件中传递ref
date: 2019-08-18 10:56:40
tags: [前端, react]
categories: [react]
description: 如何在高阶组件中传递ref
---

在antd的基础上封装，做一个叫高阶组件封装，只是多了一个层。但是却拉了一个ref，ref通过不了这个层。

首先先来回顾高阶组件的写法
```js
const Hoc = (Component) => {
  return class Wrapper extends React.Component {
    // xxxxxx

    render () {
      // 在这里做操作
      <Component {...this.props} />
    }
  }
}

export default Hoc(Input);
```

这个时候，你是用高阶组件包装后的Input发现this上并没有input的属性。这个时候才想起来ref不能通过props继续向下传下去。赶紧去找解决方案，发现React对于这个问题有相应的处理办法：[Forwarding Refs](https://reactjs.org/docs/forwarding-refs.html)

```js
const Hoc = (Component) => {
  class Wrapper extends React.Component {
    // xxxxxx

    render () {
      // 在这里做操作
      <Component {...this.props} />
    }
  }

  return React.forwardRef((props, ref) => {
    return <Wrapper {...props} ref={ref} />;
  });

}
```
在这里ref会存在在props中，但是不知道会不会有性能影响


真的需要强调ref的用途：
* Managing focus, text selection, or media playback.
* Triggering imperative animations.
* Integrating with third-party DOM libraries.

在代码中，经常会看到乱适应ref的情况，明明可以用过react本身就可以解决，非要用ref...


#### 二次封装组件库上线后的问题
1. 一定要保证和现有的样式兼容，因此样式是一个增量的增加，不是直接改，因为项目太大，如果全部替换不太现实。
2. props的增加也尽量保持灵活性。例如我在做modal的时候，我认为关闭按钮是一定存在的，上线后，发现有很多modal是自己增加的关闭按钮，于是和我的按钮就出现了重影。
3. 像button这样的组件，我实际开发的时候，可能直接用的伪类hover、active多一点，忘了focus了=.=导致上线后，样式有些瑕疵
4. 另外就是在封装组件时，没考虑到的问题，例如上面的ref问题。所以上到测试后，要跟着业务测试一下。以防想不到的情况。
5. 还有一个最难受的一个，组件库是用ts，有很多类型啥的，我是第一次见，但是解决不了就不能compile，第一次input中遇到ts的问题，修了好久==

PS: 发现的一篇精品文章[Data Structures in JavaScript: Arrays, HashMaps, and Lists](https://adrianmejia.com/data-structures-time-complexity-for-beginners-arrays-hashmaps-linked-lists-stacks-queues-tutorial/#Sets)
