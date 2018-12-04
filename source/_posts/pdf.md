---
date: 2018-04-08 21:29:01
title: 使用pdf.js在页面中在线加载pdf文件
tags: [nodejs]
categories: [nodejs]
description: 在webpack中集成pdf.js
---
在网页中浏览pdf文件，最简单的办法当然是插入iframe了
#### html5中浏览pdf文件
html5中有标签可以插入pdf,但是在每个浏览器上的表现形式不一样...
```html
<embed src="pdfFiles/interfaces.pdf" width="600" height="500" alt="pdf" pluginspage="http://www.adobe.com/products/acrobat/readstep2.html">
```

### 插件pdf.js
[pdf.js](https://github.com/mozilla/pdf.js)是一个浏览器兼容的插件，移动端也很使用，有一点：需要引入的包太大。为此，有人又做了一个包裹[pdf.js-viewer](https://github.com/legalthings/pdf.js-viewer)。这个npm是pdf.js的打包后的版本。在使用的过程中，你会发现样式不对，此时你可以将`pdf.js`中的`viewer.scss`，拷贝的你的开发目录，而不是引入`pdf.js-viewer`的css，里面的样式已经过时了。语言包引入后，页面就会翻译，不用做其他设置。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel="stylesheet" href="./viewer-2.css">
  <link type="application/l10n" href="node_modules/pdf.js-viewer/locale/zh-CN/viewer.properties" />
  <style>
    html, body {
      height: 100%;
      margin: 0;
      padding: 0;
    }
    body {
      overflow: hidden;
    }
    #pdfjs { height: 100%; }
    #viewBookmark, #secondaryToolbarToggle {
      display: none;
    }
    pdfjs-wrapper {
      display: block;
      height: 100%;
    }
  </style>
</head>
<body>
  <div id="pdfjs">
    <!--#include virtual="node_modules/pdf.js-viewer/viewer.html" -->
  </div>


  <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
  <script src="node_modules/pdf.js-viewer/pdf.js"></script>
  <script type="text/javascript">
    $(function() {
      // window.PDFJS.locale = 'zh-CN';
      $('div#pdfjs').load('http://localhost:65534/viewer.html', function(res) {
        window.PDFJS.webViewerLoad();
        window.PDFViewerApplication.open('http://localhost:65534/sample-3pp.pdf');
      });
    });
  </script>

</body>
</html>

```

### 集成到webpack中
测试开发可以正常使用，需要集成到webpack中。
由于文件都需要用webpack打包，于是按照平常一样，将文件引入，发现控制台报错。在调试过程中，我遇到5种左右的错误，影响最深刻的一点是：`document undefined`...也是很迷，调试过程中，发现`pdf.js`种的代码执行了两遍，第二次的时候就会出这个错误。仔细翻了翻源码，在`pdf.js`中，它需要引入`pdf.work.js`文件，webpack打包时，它根本就找不到这个文件，最后的结果是只在`windows`上挂载了关于`pdf`的两个对象...
最后的最后，直接在页面上引入`pdf.js`和`pdf.work.js`，由于文件过大，你按需进行加载就可以了。

最后的最后，写了很长时间的`slim`模版，我忘了原生的标签怎么写了...导致我在引入语言包话费个很长时间...也是很迷...切莫忘本😊








