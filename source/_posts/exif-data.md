---
title: 咦，图片到浏览器上怎么会自动旋转
date: 2021-01-23 21:13:23
tags: [ocr]
categories: [ocr]
description: exif-data
---

不知道你有没有遇见过图片在电脑上打开是正的，但是使用img标签显示的时候，却被旋转了。旋转会在chrome firefox上发生，safari上没有发生旋转。同样你将图片发送给安卓和苹果手机的用户，让他们分别打开，发现，咦，两个手机的表现竟然也不一样，这是为什么呢？

首先先定位到问题的原因：[戳这里](https://stackoverflow.com/questions/42401203/chrome-image-exif-orientation-issue)chrome会读取图片的EXIF data信息，并对图片做旋转。

关于EXIF，可以查看[维基百科](https://zh.wikipedia.org/zh-cn/Exif)的介绍：
>可交换图像文件格式（英语：Exchangeable image file format，官方简称Exif），是专门为数码相机的照片设定的文件格式，可以记录数码照片的属性信息和拍摄数据。
>Exif可以附加于JPEG、TIFF、RIFF等文件之中，为其增加有关数码相机拍摄信息的内容和索引图或图像处理软件的版本信息。

因为chrome是不支持tiff的展示，我做了“tiff -> png”的转换，所以在处理上我只需要处理jpg展示的格式就可以了。我怎么把它去掉呢？

#### 直接能抹掉吗
第一反应就是这个，我读取图片的base64之后，能过固定的格式拿到exif data的图片，然后移除掉。抱着这个想法，去查找有没有对应的方案，得到的答案就否。移除信息之后造成对文件的破坏，[答案链接](https://stackoverflow.com/questions/19791494/how-to-remove-exif-data-from-image-using-javascript)

#### 使用canvas当中介
在canvas上绘制图片的时候，canvas是不读取exif信息的，所以你可以先draw，然后再导出来。

#### 在原来旋转的基础上再进行旋转
对jpg的文件的旋转，是读取exif中的orientation确定的，orientation有8种情况：
![exif data](https://res.cloudinary.com/dwudaridr/image/upload/v1611405917/blog/exif-data_1.png)

数字分别对应：
1 = Horizontal (normal)
2 = Mirror horizontal
3 = Rotate 180
4 = Mirror vertical
5 = Mirror horizontal and rotate 270 CW
6 = Rotate 90 CW
7 = Mirror horizontal and rotate 90 CW
8 = Rotate 270 CW

图片左边的 1 3 6 8比较常见，2 4 5 7涉及到图片镜像的反转，一般比较少见。
例如当orientation的值为6的时候，表示浏览器已经对图片做了90度的旋转，那么你再进行360 - 90的旋转，就可以得到原来的样子。

如何得到orientation值呢？有一个exif-js这个库做这个：
```js
import EXIF from 'exif-js'
import { decode } from 'base64-arraybuffer'

// 这里的imgSrc是图片的base64
if (imageType.indexOf('jpeg') > -1 || imageType.indexOf('jpg') > -1) {
  const arraybuffer = decode(imgSrc.split(',')[1])

  const exif = EXIF.readFromBinaryFile(arraybuffer)
  console.log(exif)
}
```

另外，mozilla的文档上也有base64转换成array buffer的[方法](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowBase64/Base64_encoding_and_decoding#Solution_1_%E2%80%93_JavaScript's_UTF-16_%3E_base64)

参考资料
* https://stackoverflow.com/questions/42401203/chrome-image-exif-orientation-issue
* https://stackoverflow.com/questions/19791494/how-to-remove-exif-data-from-image-using-javascript
* https://stackoverflow.com/questions/7584794/accessing-jpeg-exif-rotation-data-in-javascript-on-the-client-side
* https://developer.mozilla.org/zh-CN/docs/Web/API/WindowBase64/Base64_encoding_and_decoding#Solution_1_%E2%80%93_JavaScript's_UTF-16_%3E_base64
* https://www.impulseadventure.com/photo/exif-orientation.html
* https://www.impulseadventure.com/photo/lossless-rotation.html
