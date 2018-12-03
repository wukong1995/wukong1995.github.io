---
  title: title
  date: 2016-06-08 20:02:41
  articleTitle: css3中-webkit-text-size-adjust
  tags: ['css']
  categories: ['前端', 'css']
  description:
---

1、当样式表里`font-size<12px`时，chrome浏览器里字体显示仍为12px，这时可以用 `html{-webkit-text-size-adjust:none;}`

2、`-webkit-text-size-adjust`放在body上会导致页面缩放失效

3、`body`会继承定义在`html`的样式

4、用`-webkit-text-size-adjust`不要定义成可继承的或全局的


