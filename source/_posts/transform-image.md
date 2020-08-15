---
title: 基于大旋转角度摆正图片的血泪汗
date: 2020-08-15 12:32:10
tags: [javascript]
categories: [前端, javascript]
description: 终于完成了
---

### 效果示例
1. 原图
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1597467374/blog/transform-img-before.png)
2.旋转后的图
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1597467374/blog/transform-img-after.png)

我要做的是将有旋转角度的图片摆正，目前只涉及大角度（90 180 270）的摆正，对于180，只是反转一下，长和宽还是原来的，我不需要对它进行太多的处理。但是对于90和270，由于旋转后，长变成宽，宽变成长，还必须将图片放在可视区域内，所以就必须调整原图片的宽高，使旋转后正好在可视区域内。业务上还有图片内容的标示，即使旋转后，我还需要记住原坐标在显示图片上的对应关系。

### 方案1
刚开始的方案是，将图片直接按照图片原点旋转，原图假如是比较长的图片，旋转后，必定有一部分不在可视区域内，我需要再进行平移、调整宽高的操作，对于图片上的标示也同样进行一样的操作，实际做起来特别麻烦，于是在想有没有更好的办法

### 方案2
返回的图片标示的坐标按照旋转后的图片的左上角为原点，这样的话，我前端只需要将图片摆正即可，标示坐标我只需要进行缩放率的计算即可。

我发现了一个很好的demo：[https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)，上面明确的标示出来旋转的原点，说起来会更清晰。这个例子也正好和我的场景类似，为了效果明显，我需要将蓝色的部分调整成长方形（`160*200`），这样它旋转`90/270`度后，会变成（`128*160`）

#### 1. 90度的处理
由于中间涉及到调整蓝色块的宽高，旋转的原点是基于按照调整后的宽高，而不是原来的宽高。

1. 首先调整宽高：`160*200` -> `128*160`
2. 调整旋转原点，增加旋转角度
`transform-origin`的默认值是 `center center`，所以调整后的旋转原点具体坐标在：`64px 80px`; 这样旋转后，调整后的高并没有在虚线框内，所以需要将旋转原点的`x-offset`往外挪一点，调整为宽的一半，这样保证旋转后的高正好在虚线框内。这个时候再进行旋转`transform: rotate(90deg); `，会发现效果刚刚好；

#### 2. 270度的处理
当时刚开发的时候，想着270不就是-90吗，效果应该和90一样，但是直到看到效果，才发现，旋转角度是反的，-90旋转后，是靠在原来高的下方，这样离视图区域的上方有一定距离。这个时候想了两个方法：

1. 调整原来的方案，不调整旋转原点，90度和270旋转后，是在同一个位置，然后再使用margin去调整
2. 再90度方案的处理上，再进行270度的处理

现在想来很清晰，方案一的margin的调整就等于`（调整后的高 - 调整后的宽）/ 2`，当时自己没想清楚，没找到公式，于是采用的第二个方案。旋转270后，你会发现它是靠在高的底部，于是我只要调整margin-top，将它挪到高的顶部即可，中间的距离就是`调整后的高 - 调整后的宽`。

### 总结
今天总结下来很清晰，当时想方案的时候特别模糊，本来我一直以为原点是基于调整之前，后来自己研究的时候才发现是基于调整后的宽高进行的。另外业务里面的是图片，并不知道宽高，所以还需要进一步的计算。总的下来：

```js
// 90/270的旋转
let height = 'auto'
let transformOrigin = 'center center'
let marginTop = 0
if (imageAngle === 90 || imageAngle === 270) {
  // 1. 先得到图片当前的显示宽度
  const { width: preShowWidth } = this.$img

  // 2. 设置图片的宽度，以得到原始的高度和宽度
  this.$img.style.width = 'auto'
  const { height: realHeight, width: realWidth } = this.$img

  // 3. 调整后的高度是原来的宽度
  height = preShowWidth
  ratio = preShowWidth / realHeight

  // 4. 设置原点的x-offset 为调整后的高的一半
  transformOrigin = `${height / 2}px center`

  // 假如旋转角度是270，需要调整marginTop = -(showHeight - showWidth)
  // 由于在设置高度之前我是拿不到图片显示的宽度，于是引入了一个缩放比
  // showWidth = realWidth * ratio（图片的缩放比）
  if (imageAngle === 270) {
    marginTop = (realWidth * ratio) - preShowWidth
  }
}
```
