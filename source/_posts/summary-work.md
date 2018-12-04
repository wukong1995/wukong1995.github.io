---
date: 2018-07-14 11:34:28
tags: ['react']
categories: ['总结']
title: 最近发现问题的汇总
description: 化繁为简，什么才是简？
---

#### react selector
最近我看到很多组件需要的redux中的值是一样的，但是，我需要为每个组件写一个mapState，我最直观的感觉是代码的重复性。于是我看了文档，在redux中的找到了一个selector的概念，看了看是可以减少我的代码重复性的。我尝试写了一个，就将这个重构工作交给同事。
同事给我的说的意思是：selector可以理解成vue里面的computed。是通过计算得到一个新的值。
例如：
```js
const getCurrentUser = (state) => ({
  currentUser: state.info.currentUser
});
```
虽然上段代码需要在多个组件中出现。如果抽成一个方法，那么阅读代码的人需要再去找到这个方法才知道我实际需要的数据格式，虽然函数的名字已经很明确，但是你不清楚实际上在组件中使用的变量名。

例如：
```js
const searchSelector = {
  hasSearchFunction: (state) => state.info.currentUser.isBig && state.info.currentUser.isSmall
}
```
上段代码体现了selector的特点，通过计算得到一个新的值。这是一个提倡的做法。

#### redux store
虽然这个坑我没遇到过，但是踩坑的人告诉我，不要什么东西都放在store里面，在store里面存值，是要进行一次stringfy，最直接的就是不要把不能进行stringfy的数据往store里面放。

#### 组件大小
最近在开发新功能的时候，会看到之前写的代码。我能体会得到之前组长的槽点。现在我看到组件，不光大而且里面很多if，看着就脑袋疼。React提倡组件可复用性。组件如果做到最大的复用性，最直接的方法就是组件功能单一性，这样组件可能变得非常小，最好还是把握一下度。
以下代码会给你什么启示？
```js
// ArticleItem.jsx
const ArticleItem = () => {
  return (
    <div className="article-item">
      <div className="article-item__cover">
        {/* ....content */}
      </div>
      <div className="article-item__conent">
        <div className="article-item__content--top">
          {/* ....content */}
        </div>
        <div className="article-item__content--middle">
          {/* ....content */}
        </div>
      {/* ....others content */}
      </div>
      {/* ....content */}
    </div>
  );
};

// ArticleItem.jsx
const ArticleItem = () => {
  return (
    <div className="article-item">
      <ArticleItemCover />
      <ArticleItemContent>
        <ArticleItemContentTop />
        <ArticleItemContentMiddle />
      </ArticleItemContent>
    </div>
  );
};
```
以上代码会给你带来不同的直观感受。阅读第一版代码，需要足够的耐心来阅读才能知道article中的内容。看第二版的代码，你可以很清晰的看到，article中到底有着什么内容。
嗯，可以理解为什么别人不想看我的代码了，又臭又长！现在看来重构别人的代码也是件痛苦的事情。

#### 闲谈
之前看到很多的大神的经验谈，但是最真实的感受就是：没经历过真的很难去理解其中的意思。只有真正上手后，才能理解其中的奥妙。
hhhh，是不是特别像高中的老师痛心疾首的告诉学生要好好学习，当时真的内心毫无波动，但是现在在想，会发现已经没有足够的精力对待学习这个事情了。
活到老学到老，加油，你是最棒的！


