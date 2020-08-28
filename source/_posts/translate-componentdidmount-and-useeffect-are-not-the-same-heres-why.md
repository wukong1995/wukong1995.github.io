---
title: 翻译：ComponentDidMount和useEffect是不一样的，为什么
date: 2020-08-28 19:48:25
tags: [翻译, javascript]
categories: [翻译]
description: ComponentDidMount and useEffect Are Not The Same. Here’s Why
---

[**原文链接**](https://medium.com/javascript-in-plain-english/componentdidmount-and-useeffect-are-not-the-same-heres-why-cea02f474c82): https://medium.com/javascript-in-plain-english/componentdidmount-and-useeffect-are-not-the-same-heres-why-cea02f474c82

当组件挂载的时候进行一些设置是很常见的，例如网络请求。在hooks没有出现的时候，我们可以使用`componentDidMount`方法。在过渡到函数组件的时候，自然会寻求等效的hooks。

hooks和声明周期基于不同的原则。诸如`compomentDidMount`之类的方法是围绕声明周期和渲染时间展开，而hooks则是围绕state和与DOM的同步设计的。

许多程序员认为使用`useEffect(fn, [])`可以代替`componentDidMount`，虽然这不会导致大的错误产生，但是它仍会导致一些破坏程序的错误。两种方法在根本上是不一样的，你可能会得到预料之外的错误。程序员不应该认为hooks是组件挂载执行的方法。假如hooks以这样的方式工作，那么会影响你理解hooks。

#### state和props的捕获不一样
最明显的不用在使用异步方式的时候

```js
class App extends Component {
  state = {
    name: ''
  }

  componentDidMount() {
    someResolve().thene(() => {
      console.log(this.state.name)
    })
  }

  onChange = (event) => {
    this.setState()
  }

  render() {
    return (
      <input value={this.state.name} onChange={this.onChange} />
    )
  }
}
```
这个组件看起来很简单，一旦它挂载了，它会调用一个异步函数，一旦异步函数成功执行，会将当前state的name值打印出来。让我们尝试将代码移植到函数组件上。

```js
const App = () => {
  const [name, setName] = useState('')

  useEffect(() => {
    someResolve().then(() => {
      console.log(name)
    })
  }, [])

  const onChange = (event) =>{
    setName(event.target.value)
  }

  return (
    <input value={name} onChange={onChange} />
  )
}
```

上面的代码无法满足。当组件被创建，`useEffect`方法会捕获state和props的值，结果，用户即使输入了内容，console仍会打出一个空行。要告诉React使用最新的值，你必须将依赖项传给effect。props也是一样的逻辑。在这个例子中，effect比必须要使用`compomentDidMount`的class组件简单。

#### 方法在不同的时刻被调用
React可以确定何时在`componentDidMount`中同步状态，让我们看一下组件的实际生命周期：
1. 组件被挂载
2. 根据render中返回的内容创建DOM
3. `componentDidMount`被调用，state被更新
4. DOM重新渲染，render中return的内容更新

可能有人希望我们在第一帧和第二帧中间看见闪烁，但是不会出现这样的情况，React检测到状态更新后，只打印第二帧。如果组件需要在DOM渲染时才计算元素的比例，这个是很有用的。

在组件挂载之后，hooks和useEffect都会执行。不同的是：hooks在DOM内容绘制之后执行。所以，如果在effect方法中同步更新state，用户会看到第二帧替换第一帧的闪烁。

你可以使用`useEffectLayout`获取带有hooks的旧行为，`useEffectLayout`在内容commit到页面前被调用。然而大多数都不会用到这个hook，大多数程序员仍坚持使用useEffect。

class组件围绕生命周期和渲染时间设计。函数组件旨在state和DOM的同步。思维方式的转变可能会导致一些奇怪的怪癖和错误，如果没有合适的知识很难去解决。简而言之，应该考虑“基于state，我的组件应该是什么样子，以及合适应该重新渲染”，这些问题将确保你的函数组件正常运行。


