---
  title: lineheight-vertical-middle
  date: 2016-06-27 19:37:17
  articleTitle: lineheight使图片多行文字垂直居中
  tags: ['css']
  categories: ['css', '前端']
  description:
---


多行文字垂直居中

```html
<p class="mulit_line" style="width: 500px">
    <span style="font-size:12px;">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nemo voluptatum beatae officiis doloribus culpa et autem velit voluptatem quidem non, tempora, pariatur veritatis quaerat. Iste nisi nemo omnis, repellendus facilis.Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nemo voluptatum beatae officiis doloribus culpa et autem velit voluptatem quidem non, tempora, pariatur veritatis quaerat. Iste nisi nemo omnis, repellendus facilis.
    </span>
    <i>&nbsp;</i>
 </p>
```

```css
.mulit_line{line-height:150px; border:1px dashed #cccccc; padding-left:5px;}
.mulit_line span{display:-moz-inline-stack; display:inline-block; line-height:1.4em; vertical-align:middle;}
.mulit_line i{width:0; display:-moz-inline-stack; display:inline-block; vertical-align:middle; font-size:0;}
```

```html
<ul class="zxx_ul_image">
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm1.jpg" /></li>
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm2.jpg" /></li>
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm3.jpg" /></li>
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm4.jpg" /></li>
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm5.jpg" /></li>
  <li><img src="http://image.zhangxinxu.com/image/study/s/s128/mm6.jpg" /></li>
</ul>

```css
.zxx_ul_image{overflow:hidden; zoom:1;}
.zxx_ul_image li{float:left; width:150px; height:150px; text-align:center; line-height:150px; *font-size:125px;}
.zxx_ul_image li:after{content:' '; vertical-align:middle;}
.zxx_ul_image li img{vertical-align:middle;}
```
转载而来。。。


