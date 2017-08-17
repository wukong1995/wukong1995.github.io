---
title: bootstrap-modal
date: 2018-08-08 13:29:51
articleTitle: bootstarp中的modal打开时为神马动态为body增加padding-right
tags: ['前端']
categories: ['前端']
description: bootstarp中的modal打开时为神马动态为body增加padding-right
---

之前看`bootstrap`的modal的时候，当modal打开时，动态为`body`增加`padding-right: 15px`的style，刚开始时有点疑惑为什么不直接给`open-modal`增加`padding-right: 15px`，还非要动态增加呢？

![image](http://res.cloudinary.com/dwudaridr/image/upload/v1533706275/blog/2091533706051_.pic_hd.jpg)

由于现在的项目中也有modal，我是采取的第二个方案，通过class给body增加样式。由于我的chrome的滚动条是需要占空间的，emmm，打开modal，样式是ok的，但是测试给我说，打开modal的时候页面整体会移动，很疑惑，看了他的浏览器，发现浏览器的滚动条是不占宽度的，所以当打开modal的时候，给`body`增加`padding-right`导致页面右移。

这个时候，我赶紧去看`bootstrap`的源码：
```js
// modal.js
Modal.prototype.show = function (_relatedTarget) {
  // .....
  this.checkScrollbar()
  this.setScrollbar()
  this.$body.addClass('modal-open')
  // .....
}

Modal.prototype.checkScrollbar = function () {
  var fullWindowWidth = window.innerWidth
  if (!fullWindowWidth) { // workaround for missing window.innerWidth in IE8
    var documentElementRect = document.documentElement.getBoundingClientRect()
    fullWindowWidth = documentElementRect.right - Math.abs(documentElementRect.left)
  }
  this.bodyIsOverflowing = document.body.clientWidth < fullWindowWidth
  this.scrollbarWidth = this.measureScrollbar() // 得到scoll bar的宽度
}

Modal.prototype.measureScrollbar = function () { // thx walsh
  var scrollDiv = document.createElement('div')
  scrollDiv.className = 'modal-scrollbar-measure'
  this.$body.append(scrollDiv)
  var scrollbarWidth = scrollDiv.offsetWidth - scrollDiv.clientWidth
  this.$body[0].removeChild(scrollDiv)
  return scrollbarWidth
}

Modal.prototype.setScrollbar = function () { // 为body动态增加padding-right
  var bodyPad = parseInt((this.$body.css('padding-right') || 0), 10)
  this.originalBodyPad = document.body.style.paddingRight || ''
  if (this.bodyIsOverflowing) this.$body.css('padding-right', bodyPad + this.scrollbarWidth)
}

// 注： 如果没有对body的宽度做设置
// fullWindowWidth - document.body.clientWidth也可以得到scrollbar的宽度
```
每次打开modal的时候，动态的计算一下scrollbar的宽度，宽度即为`padding-right`的值。关闭modal的时候清空style的值即可。

#### 扩展
1. 先去增加body的style，再增加class，在写的过程中，我一直得到scrollbar的width为0，我还想着是不是异步的计算，检查了代码以后，发现是先加class再计算才导致每次计算为0。

2. 我测试了Chrome、firefox、safari三个浏览器，scrollbar的width都是15，看的资料说scrollbar的width一般在14-18之间...

3. 我检索了一下：如何隐藏chrome的滚动条，得到的答案是： `chrome://flags/#overlay-scrollbars`，我去找这个设置的时候，发现已经找不到这个设置了.....
