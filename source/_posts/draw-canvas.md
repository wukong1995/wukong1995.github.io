---
title: 高亮文件中识别出的文字结果最佳实践 - canvas
date: 2021-02-28 14:05:39
tags: [ocr]
categories: [ocr]
description: 无敌canvas
---

对于一个文件，ocr识别后，会吐出文件中所有文字的坐标，结合图片标示出来，目前就是这样的效果：
![image-highligh](https://res.cloudinary.com/dwudaridr/image/upload/v1614494956/blog/image-highlight.png)

### step1: 只标示图片
这个好办啊，只有图片，一个image搞定，标示高亮用div绝对定位展示

```html
.container
  .inner
    img src="xxxx"
    .highlight1 style="xxxx"
    .highlight1 style="xxxx"
```

遇到需要旋转的图片也能搞定，给img增加`transform`的style就可以搞定，参照我之前写的[基于大旋转角度摆正图片的血泪汗](https://wukong1995.github.io/2020/08/15/transform-image/)

### step2: 增加pdf的展示，但是可以只展示第一页哦
在原来的基础上增加了一个新的类型，那么可以延续之前的方式，使用`pdf-dist`将pdf的第一页渲染到`canvas`上，再用`canvas`转化成图片的base64

```js
const viewport = page.getViewport({ scale: 1 })
const context = $canvas.getContext('2d')
$canvas.height = viewport.height
$canvas.width = viewport.width

const task = page.render({ canvasContext: context, viewport })
const base64 = $canvas.toDataURL('image/png')
```
这样就可以支持pdf，`canvas`在里面只充当了一个中间商，觉得有些可惜，也没有再扩展别的方法。

总之是决定img一条路走下去，里面的路已经走的差不多了

### step3: 支持整个pdf的支持
从step2走下去，按照原来的方式继续做，实现的非常ok。但是我详细的了解一下，pdf现在是10页的规格，以后说不准有多少...顿时傻眼了

目前的highlight都是使用的`div`表示的，一页保守计算是20个，10页的就是200个div，按照现在的方式，太多的dom元素会有性能问题。所以去看看还有没有更好的方式。

`canvas`真是及时雨，它什么都可以render。那我直接画上highlight岂不是一个canvas元素就搞定了，使用`canvas`代替`一个img+n个highlight`。在step2的基础上加上：

```js
const ctx = canvas.getContext('2d');

// 1. 清除canvas
ctx.clearRect(0, 0, canvas.width, canvas.height);
// 2. render pdf
// 参照step2的code

// 3. render highlight
ctx.fillStyle = 'rgba(87, 87, 244, .3)';

highlightList.forEach(item => {
  const { top, left, width, height } = item;
  ctx.fillRect(left, top, width, height);
});
```

另外对于图片的展示，canvas也是手到擒来，旋转的图片也可以搞定。这样比使用style摆正图片也简单一些。
```js
const image = new Image();
image.onload = () => {
  const { naturalWidth, naturalHeight } = image;
  const scale = width / naturalWidth;
  $canvas.width = width;
  $canvas.height = naturalHeight * scale;
  ctx.drawImage(image, 0, 0);
};
image.src = imgBase64;
```

另外在canvas里面画带框的矩形
```js
ctx.strokeStyle = borderColor;
ctx.strokeRect(left, top, width, height);
```

这里要注意的问题是，
1. 对`canvas`设置`width: 100%; height: 100%;`的效果是在原来`300*150`基础上拉伸，所以一定要等canvas设置好宽高再进行draw
2. 本来使用一个canvas来表示一个pdf，但是canvas有最大的宽高`32767*32767`。我图省事，将canvas的高设置称为200000，结果发现chrome上canvas只剩一个哭脸图标。参见[详情](https://www.tutorialspoint.com/Maximum-size-of-a-canvas-element-in-HTML)

### 总结
step3中决定使用canvas全部代替image是一个好的决定。其实2中就应该拥抱canvas，可谁知道当时怎么想的。使用每页渲染的方式，以后pdf页数多了，也可以使用按需加载进行优化。

所以遇到巨多的dom节点时候，看是否可以使用cavas去代替！！！
