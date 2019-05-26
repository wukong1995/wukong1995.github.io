---
title: 如何更改antd的样式
date: 2019-05-19 00:19:26
tags: [javascript]
categories: [前端, javascript]
description: 设计小姐姐会很开心的～
---


新地方用的是antd组件，功能可以满足需求，但是样式需要调整一下，找到了两个方法：一个是粗犷的暴力覆盖样式，另一个是通过改变less里面的值进而达到更改样式

方法1:
```css
@import '~antd/lib/modal/style/index';

.ant-modal {
  &-header {
    border-bottom: none;
  }

  &-title {
    color: red;
  }
}

```


方法2:
**因为之前使用的是sass预处理，变量支持default和import，但是less是不支持这个操作的**，想要达到目标，我只能采取变量覆盖的方法，这就需要在引入定义变量的文件后面覆盖变量的值（和定义变量相同），这样就导致一个问题：我必须要了解组件style文件中的逻辑。这样一来，我的自定义组件中的样式架构就需要跟着antd的变化而改变。

```css
@import '~antd/lib/style/themes/default';
@import '~antd/lib/style/mixins/index';

@border-width-base: 0;
@heading-color: red;

@import '~antd/lib/modal/style/modal';
@import '~antd/lib/modal/style/confirm';
```

目前只想到这两个方法。

但是比较伤的是我发现项目中的antd版本不一样，那么就要意味着我写两个版本的组件样式库？？？

PS: 今晚等雨的我，怕是等不到雨了...

