---
title: 终于要使用css modules
date: 2020-04-19 16:33:09
tags: [css]
categories: [前端, css]
description: 这应该是一年前想做的最后一个todo了
---

项目的庞大以及glamor的年久失修，不得不使用css modules了。我非常不喜欢css in js，它只会让css失去原有的魅力，仅仅是为了解决命名冲突的问题，搞得css书写特别困难，为了实现类似keyframe，而不得不做各种各样的api来规避它。用了它之后把整体的样式书写的乱七八糟，无尽的嵌套简直是噩梦。虽然不是UI工程师，但是也期待用最少的css代码来实现最好的样式。

css module的出现，简直是一道把人拉到正道上的曙光。具体的配置参见[create-react-app的react-script](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/config/webpack.config.js)

说一说的我的使用感受（我是使用的less预处理）：
```css
.container {
    .title {
        // ....
    }
}
```
如果这样写，styles中会有两个属性container和title，其实刚开始特别不了解，但是仔细想了想，现在都是用bem来命名，用bem正常的写法
```css
.xxx {
    &-container {
        // ...
    }

    &-title {
        // ....
    }
}
```
上面的写法能用的class应该是两个：xxx-container和xxx-title，所以这样暴露没什么问题。那如果你不想将上上面的title暴露出来怎么办，那这个时候你要用到global了
```css
.container {
    :global {
        .title {
            // ....
        }
    }

    &:global {
        &:hover .title {
            // ...
        }
    }
}
```

关于gloabl的用法，没找到太详细的资料。global指的是全局作用域，其实我理解的是：当前环境的全局作用域，即title用global包起来之后，title样式的生效还是依赖于container作用范围的。`:global`和`&:global`的区别不知，你如果将它们交换一下，那么css-loader一定会报错。

还有一个问题是css module中引用图片一定要以`~`开头。项目中自定义的alias也是以这个符号开头，但是`~~static/image/xxx`会导致图片找不到，无奈只能将`~static`改为`@staic`

参考资料
* [github css modules](https://github.com/css-modules/css-modules)
* [Stop using CSS in JavaScript for web development](https://medium.com/@gajus/stop-using-css-in-javascript-for-web-development-fa32fb873dcc)
* [CSS Modules使用详解](https://imweb.io/topic/586519b1b3ce6d8e3f9f99aa)

