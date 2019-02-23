---
title: 我的对scss的错误认知
date: 2019-02-23 18:57:23
tags: [css]
categories:
  - [css]
  - [前端]
description: scss真的是很简单的么
---

之前的post介绍的重构的评论组件，我的重构工作仅仅从coding的复杂度去考虑，却没有进行深入的研究，例如渲染原理等等。用react实现实际上是不是比重构后更快，毕竟算法的复杂度已经降到O(n)了。这是一个很大的失误。

继上面的错误之后，有犯了一个很粗鄙的错误。没有正确的认识到scss作为css的预处理的作用。目前项目中scss的作用只用到变量定义和嵌套的。在我查看bootstrap的源代码的时候，粗略的看了一下，嗯，感受到的是一种形散神不散的感觉，就像它是一张网，但是你只是将它们看成一条一条的线。这个时候赶忙去看scss的doc，才发现它既然是一种预处理的语言，相对于css，它必然包含更多更好的功能。

###### 变量
变量也可以进行计算哦。

```sass
$red: red;

body {
  color: $color;
}
````

###### 嵌套
```sass
.btn {
  color: #fff;

  &.t-red {
    background: $red
  }
}
```

###### namespace
```sass
.btn {
  font {
    size: 1.6rem;
    weight: normal;
  }
}
````

###### each and map
这个功能真的很棒很棒，超级棒！棒到我都不想写之前的code。

```sass
$theme-colors: (
  'blue': $blue,
  'pink': $pink,
  'green': $green,
  'yellow': $yellow,
  'red': $red
);

.badge {
  color: #fff;

  @each $color, $value in $theme-colors {
    &.t-#{$color} {
      background-color: $value;
    }
  }
}
````

###### function
看到设计图里面很多颜色都是带透明度的，但是场景是不需要用到rgba，于是写了一个将带有透明度的颜色转化成不带透明度的颜色，带透明的颜色实际上是和白色以透明度去混合得到。

```sass
@function rgba-to-rgb($rgba, $background: #fff) {
  @return mix(rgb(red($rgba), green($rgba), blue($rgba)), $background, alpha($rgba) * 100%);
}
````

##### 避免使用@extend
之前看到的文章里面专门来讲这个事情，不要使用@extend而是使用@inlcude，举个栗子：

```sass
h3 {
  font-size: 1.8rem;
}

.home h3 {
  font-size: 2rem;
}

.h3 {
  @extend h3;
}
````

这时候得到的css是：
```css
h3, .h3 {
  font-size: 1.8rem;
}

.home h3, .home .h3 {
  font-size: 2rem;
}
````
你仅仅想要继承的是1.8的这个样式，但同时也把2的样式给继承了。

还有更多更好的功能去探索。
