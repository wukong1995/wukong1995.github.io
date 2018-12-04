---
date: 2018-10-22 18:32:13
title: stimulus初体验
tags: ['typescript']
categories: ['typescript']
description: stimulus初体验
---

#### demo
[stimulus](https://github.com/stimulusjs/stimulus)
ts作为js的超集，应用率也是非常广泛，在github上看到一个有趣的框架，于是想着从hello word走起，看完栗子有，我尝试写一个列表...

```html
<div data-controller="content-loader" data-content-loader-url="/message.html" data-content-loader-refresh-interval="5000"></div>

  <h1>list controller</h1>
  <div data-controller="list" data-list-initial='["name"]'>
    <div>
      <input type="text" data-target="list.text" value="hello1">
      <button data-action="click->list#add">add</button>
    </div>
    <div data-target="list.items"></div>
  </div>
```

```js
import { Controller } from 'stimulus';

export default class extends Controller {
  static targets = ['text', 'items', 'item'];

  connect() {
    this.getInitialItems();
  }

  get items() {
    return this.items;
  }

  set items(newItems) {
    this.items = newItems;
    this.render(newItems);
  }

  get text() {
    return this.textTarget.value;
  }

  getInitialItems() {
    try {
      this.items = JSON.parse(this.data.get('initial'));
    } catch(e) {
      this.items = [];
      console.error(e)
    }
  }

  render(items) {
    this.itemsTarget.innerHTML = items.reduce((acc, item, index) => {
      return acc + `<p data-target="list.item"><span>${item}</span><button data-id="${index}" data-action="click->list#deleteItem">delete</button></p>`;
    }, '');
  }

  add() {
    this.items = this.items.concat(this.text);
  }

  deleteItem(event) {
    const targetIndex = event.currentTarget.dataset.id;
    const newItems = [
      ...this.items.slice(0, targetIndex),
      ...this.items.slice(targetIndex + 1)
    ];
    this.items = newItems;
  }
}
```

看起来非常good，于是开始运行，发现死循环了，检查了代码没发现错误，尝试debugger发现程序一直在执行set操作，很纳闷，没执行set呀。找机会问了哟哟，他告诉我，在set中尝试给this.item意味着你在set调用set本身，于是乎，死循环了...

```js
get items() {
    return this._items;
  }

  set items(newItems) {
    this._items = newItems;
    this.render(newItems);
  }
```
修改成以上代码可以正常工作。

#### 思考：
1、如果以上面的方式进行，我觉得还不如做成面向对象中的set/get....这只是我个人的想法
2. 将html写在js中一点都不优雅，我找到了一个[demo](https://github.com/stimulusjs/stimulus/issues/41)，它提供一个更好的方式。
3. 使用stimulus，思维方式和传统的js相似
4. 在stimulus中，数据和元素不是一一对应的，所以在deleteItem中直接删除元素并且改变items的方式会不会比通过数据去改变dom更好一点？虽然二随一变会容易出bug，但是考虑到列表可能会很多，删除一个元素比重新渲染整个列表代价小很多吧.....





