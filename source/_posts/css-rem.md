---
  title: css中rem的注意点
  date: 2018-10-25 19:22:18
  tags: [css]
  categories:
    - [css]
    - [前端]
---

今天在使用rem中发现了一个之前没注意的点
```css
html {
  font-size: 10px;
}

.title {
  font-size: 1.6rem;
}

.container {
  width: 2.4rem;
}
```

其中title的字体大小为16px，但是container的宽度却是28.8px，我算了一下28.8/2.4=1.2，想到浏览器的最小字体是12px，所以我想着可能和最小字体的大小有关系。

于是我尝试将chrome的最小字体设置为最小（比12px还小），发现container的宽度是24px。

去查资料没有找到相应的资料，可以了解的是rem最好使用在font-size上，使用在margin和padding上也会出现和期望不用的值。
