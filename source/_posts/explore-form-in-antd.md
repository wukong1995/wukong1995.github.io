---
title: 提取公共组件-探索antd中的form
date: 2019-07-06 21:42:21
tags: [前端, react]
categories: [前端]
description: 包装组件的时候遵循单向数据流
---

目前遇到的问题是：有一个内部逻辑很复杂的公共子组件，对于它我的期望是我传给他一个初始值，随它内部怎么操作，最后我只要得到结果即可。这就要求数据和逻辑全部在自组件内部，但是我怎么在父组件里面得到子组件内部的数据？这似乎是违背了react单向数据流的思想，数据可以从父组件流向子组件，但不可以从子组件流向父组件。如果使用状态提升，这会导致我会在父组件里面创建state，写很多methods，既然这是一个公共组件，那每次使用一次我就在父组件里面增加一遍数据和函数吗？这似乎也不是最佳实践。这时候我想到了antd中的form。

在使用的时候你会注意到
```js
Form.create({ name: 'normal_login' })(NormalLoginForm);
```
这里可以看出来`Form.create`会返回一个高阶组件，在`NormalLoginForm`里面得到form中的值也是通过props.form得到的。form里面有一个方法`getFieldDecorator`，文档的定义是：用于和表单进行双向绑定。为什么要进行双向绑定呢？form中的数据其实是存在高阶组件里面的，高阶组件和Input之间隔了一个你自己的组件，想到这里似乎有些明朗了，双向绑定是绑定`Input`和高阶组件的`state`。

我想了一下这个逻辑是可以解决我的问题的，于是想看一下antd是如何实现的
1. `Form`中`create`是`static`函数，它返回了一个`createDOMForm`高阶组件。`mapPropsToFields`是每次在`getInitialState`和`componentWillReceiveProps`中更新state的值

2. getFieldDecorator
```js
getFieldDecorator() {
  // ...
  return React.cloneElement(fieldElem, {
    ...props,
    ...this.fieldsStore.getFieldValuePropValue(fieldMeta),
  });
}
```
于是照着上面的步骤，写了一个高阶组件和公共组件，这个时候我在双向绑定上犯了难，antd是写了一个类似redux的store，有点闹不懂的是没有将数据放在state里面，组件如何重新render...因为目前业务需求不是很复杂，所以换一种思路。

#### 实现
##### 1. 创建一个公共组件
数据都是通过props来的

```js
import React, { Fragment } from 'react';

const Reply = (props) => {
  const { list, delItem, addItem } = props;

  return (
    <Fragment>
      <div className="list">
        <p>这里是reply</p>
        {
          list.map((item, index) => (
            <div key={index} onClick={() => delItem(index)}>{item}</div>
          ))
        }
      </div>
      <button onClick={addItem}>add</button>
    </Fragment>
  )
}

export default Reply;
```

##### 2. 创建一个高阶组件

```js
import React from 'react';
import Reply from './Reply';

const Hoc = (options) => (Component) => {
  class Wrapper extends React.Component {
    constructor() {
      super()
      // get initial state by options.mapPropsToFields
      this.state = {
        list: [1, 2]
      }
    }

    delItem = (index) => {
      const { list } = this.state

      this.setState({
        list: [
          ...list.slice(0, index),
          ...list.slice(index + 1)
        ]
      })
    }

    addItem = () => {
      const text = prompt()

      if(text) {
        const { list } = this.state
        list.push(text)
        this.setState({
          list
        })
      }
    }

    getReplyCard = () => {
      return (
        <Reply list={this.state.list} addItem={this.addItem} delItem={this.delItem} />
      )
    }

    getList = () => {
      return this.state.list
    }

    render() {
      const { list } = this.state;

      return (
        <Component {...this.props} getList={this.getList}  getReplyCard={this.getReplyCard} />
      )
    }
  }

  return Wrapper
}

export default Hoc;
```

##### 3. 使用高阶组件

```js
import React from 'react';
import Reply from '../components/reply'

class ContainReply extends React.Component {
  render () {
    return (
      <div>
        <p>这是我的组件</p>
        {this.props.getReplyCard()}
      </div>
    )
  }
}

export default Reply({})(ContainReply)
```

以上方式可以实现我的需求了....
form还需再探索