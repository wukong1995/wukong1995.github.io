---
title: 浏览器中，将pdf和tiff转换成常见格式的图片显示
date: 2020-08-08 16:40:43
tags: [前端]
categories: [前端, react]
description: pdf只显示第一页，tiff格式每个浏览器支持不一样
---

最近项目，要支持多种图片类型和pdf的显示，tiff在每个浏览器中的展示是不一样的，在使用最多的chrome中，就无法显示；pdf的话，是截取第一张显示出来。

### 1. 首先使用FileReader将文件转成base64
```js
// file可能是本地选择/或者通过xhr请求到
const file = request.response
const reader = new FileReader()
reader.readAsDataURL(file)
reader.onload = () => {
  if (reader.result) {
    const readerResult = reader.result

    rsolve(readerResult, file)
  }
}
```

### 2. pdf的转换
找到了一个package（pdfjs-dist）能将pdf转换成图片，不过中间也需要canvas的支持，它可以将转换到的流画到canvas上，然后canvas再导出为图片。

1. 安装
```js
yarn add pdfjs-dist
```
2. 转换

```js
import PDFJS from 'pdfjs-dist'

// 这个cdn比较慢，可以下载到项目本地或上传至自己的cdn
PDFJS.GlobalWorkerOptions.workerSrc = '//cdnjs.cloudflare.com/ajax/libs/pdf.js/${PDFJS.version}/pdf.worker.js'

PDFJS.getDocument(readerResult).promise
    .then((pdf) => {
      // 只取第一页
      pdf.getPage(1)
        .then((page) => {
          const viewport = page.getViewport({ scale: 1 })
          const context = $canvas.getContext('2d')
          $canvas.height = viewport.height
          $canvas.width = viewport.width

          const task = page.render({ canvasContext: context, viewport })
          task.promise.then(() => {
            // 使用$canvas的api转换成图片
            setImgUrl($canvas.toDataURL('image/jpeg'))
          })
        })
    })
    .catch((error) => {
      console.error('pdf转图片失败', error)
    })
```
打开浏览器，可以顺利显示；刚上线两天，有人报bug，一看他的pdf展示的时候，丢失了很多信息。我以为是只是前端的问题，但是后端同学尝试将pdf转成图片之后，发现图片也是有问题的。另外，我找到一个外婆家的发票试了一下，发现完全正常。**所以，这有个坑，在本地正常显示的pdf，有可能无法转换成正常的图片。**

### 3. tiff
[可以点这里看每种浏览器对每种图片格式的支持情况](https://en.wikipedia.org/wiki/Comparison_of_web_browsers#Image_format_support)
Chrome、Firefox都不支持显示😭，这个必须需要解决。幸运的是，也找到了package来做这个事，它是先将file转换成ArrayBuffer而不是base64，然后再进一步转换成常见的图片格式。

1. 安装
```js
yarn add tiff.js
```
2. 转换

```js
const bufferReader = new FileReader()
bufferReader.readAsArrayBuffer(file)

bufferReader.onload = () => {
  if (bufferReader && bufferReader.result) {
    const image = new Tiff({ buffer: bufferReader.result })
    setImgUrl(image.toDataURL('image/jpeg'))
  } else {
    cosole.error('上传失败, 请重试')
  }
}
```

OK，两种文件格式都已经可以转换成常见的图片格式在浏览器中展示。

