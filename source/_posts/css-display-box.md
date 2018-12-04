---
  title: CSS3中的display(box)
  date: 2016-06-08 17:22:48
  tags: [css]
  categories:
    - [css]
    - [前端]
  description:
---


今天看源码时，看到了`display: -webkit-box;display: box`; 似乎是第一次见，
上网搜寻：

`display: box;`的声明其实就是弹性盒子模型的声明，此声明下的子元素的行为与表现与CSS2中的传统盒子模型的表现是有显著的差异的。

毕竟属于CSS3的东西，目前而言，仅Firefox/Chrome/Safari浏览器支持弹性盒子模型（IE9不详，Opera尚未），且使用的时候，需要附带私有前缀。就是诸如-moz-, -webkit-之类。

父元素使用`display:box;`子元素就可以使用box-flex瓜分父元素的地方了

1、之前要实现横列的web布局，通常就是float或者`display：inline-block; `，但是都不能做到真正的流体布局。至少width要自己去算百分比。
2.`flexible box`就可以实现真正意义上的流体布局。只要给出相应属性，浏览器会帮我们做额外的计算。

提供的关于盒模型的几个属性：
```css
box-orient           子元素排列方向，值为horizontal | vertical | inline-axis | block-axis | inherit
box-flex             兄弟元素之间比例，仅作一个系数
box-align            剩余空间如何使用
box-direction        子元素的排列顺序，值为：normal | reverse | inherit
box-flex-group       以组为单位的流体系数
box-lines            子元素是否换行显示
box-ordinal-group    以组为单位的子元素排列方向box-pack   父元素水平遗留空间的使用
```

[参考来源](http://www.zhangxinxu.com/wordpress/2010/12/css-box-flex%E5%B1%9E%E6%80%A7%EF%BC%8C%E7%84%B6%E5%90%8E%E5%BC%B9%E6%80%A7%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B%E7%AE%80%E4%BB%8B/)


