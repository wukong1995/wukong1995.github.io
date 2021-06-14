---
title: 单元测试我写的好烂
date: 2021-06-06 09:42:48
tags: [javascript]
categories: [前端, javascript]
description: 这是一个肯定句
---

### 之前的积累：可以说很少
在之前的公司，是最后才开始写测试的，那个时候写起来就感觉有点难受。我记得那个case是我尝试去点击Link，然后判断页面的url是否有改变，我刚开始尝试用侦测window.location去感知url的改变，但是history的改变，不会去触发location的事件，最后的最后，我退而其次去判断了函数有没有被调用，现在来看也不是一个好办法，判断的条件是这样才更稳妥：
```js
const spy = jest.spyOn(router, 'push')
expect(spy).toBeCalledWith('new url')
```

### 使用jest暴露的问题
这次刚进去的task也是写单测，相对之前，每个小业务都被封装在包中，react的部分也少，jest写单测的话，真的是捉襟见肘啊，而且我好像没有掌握jest的技巧，感觉写单测都是以写集成测试的思想在写，所以写出来的质量不怎么样。我新增的一个文件的测试，觉得还可以，但一看覆盖率才百分之四十多，于是我开始疯狂的补case想要把每个函数的每个分支都给覆盖了，所以一眼看上去我的测试写的特别多，特别长，但是对照其他同学写的，感觉我写的好像是特别笨重，效果不是那么大。我写了好几天的case，只提升了五个点💔，我总结一下这次遇到问题：

##### 1. 异步函数：callback形式

代码可能长这样：
```js
dynamicInsertScript('scriptUrl', (data) => {
    window.data = data
    // data.xxx
})
```
data上的属性和方法特别多，mock起来特别费劲，要是callback里面代码巨多，那针对这一个函数要写很多case。首先刚开始写的时候，我又陷入了误区，一个很朴素的想法，我在写的时候，sleep上很长时间，等待这个函数load完，window.data不就有值了吗，然后我再去判断或者触发data上的属性，于是我写下了：
```js
test('case', () => {
    setTimeout(() => {
        expect(window.data).no.toBe(undefined)
    }, 1000)
})
```

写完之后，跑过了，完美，以后这样的例子就这么写就行了，开心的提了mr，reviewer给了comment：setTimeout在这里面是无效的，需要done一下。然后我又在setTimeout的最后一行加了done，但是jest一直报错 `async callback was not invoked within the 5000ms timeout specified by jest.settimeout.`，这个时候我想到setTimeout用错地方了，我还是老老实实的mock所有函数吧。于是改成了下面这样：

```js
jest.mock('@utils', () => ({
    dynamicInsertScript: (_, cb) => {
        const data = {
            // xxx: 
        }

        cb(data)
    }
}));

test('case', () => {
    expect(window.data).no.toBe(undefined)
})
```
这样的话就没有异步了，但是如果data上有很多很多属性，如果当前测试的文件里面有用到，必须把这些属性全部mock，如果有复杂的属性，就写起来比较艰难。

##### 2. 异步函数：promise
有了第一个的经验，可不能再用setTimeout了，这个mock的想法和上面的类似，对于代码

```js
conts getList = () => {
    const data = await fetchData('xxx')
    // 下面是处理data的部分
}
```

可以这样写case：
```js
test('case', () => {
    const spy = jest.spyOn(api, 'fetchData')
    // 每次调用返回不同的值
    spy.mockImplementationOnce(() => Promise.resolve({ 
      // xxx
    }))

    // 一定要记得加await！！！
    await getList()
    spy.mockImplementationOnce(() => Promise.resolve({ 
      // xxx
    }))
    await getList()
})
```
剩下的问题和上一个一行，你要mock的数据要完整。

##### 3. setTimeoue如何快速执行
针对下面的代码：

```js
const change = () => {
    setTimeout(() => {
        // xxx
    }, 0)
}
```

可以这样来：
```js
jest.useFakeTimers();

test('case', () => {
    change()
    jest.runAllTimers()
    // 然后check结果
})
```

##### 4. 检查函数调用的参数
针对下面的代码：
```js
const tip = () => {
    message.error('error')
}
```

可以这样来：
```js
test('case', () => {
    message.error = jest.fn()
    tip()
    expect(message.error.mock.calls[0][0]).toBe('error');
})
```

##### 5. window.addEventListen

```js
test('case', () => {
    const map = {}
    window.addEventListen = (type, func) => {
        map[type] = func
    }

    window.removeEventListen = (type) => {
        delete map[type]
    }

    map.error({preventDefault: () => {}})   // 参数为event对象
})
```

##### 6. localStorage

```js
test('case', () => {
    const spyGetItem = jest.spyOn(window.localStorage.__proto__, 'getItem')
})
```

##### 7. new Date()
```js
test('case', () => {
  // jest < version 26
  const mockDate = new Date(1466424490000)
  const spy = jest
    .spyOn(global, 'Date')
    .mockImplementation(() => mockDate)

  spy.mockRestore()
})
```

##### 8. mock window伤的对象
```js
Object.defineProperty(window.document, 'cookie', {
    get: jest.fn(),
});
```

