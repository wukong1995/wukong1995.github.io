---
title: 京东分类栏
date: 2016-05-18 14:42:51
tags: [javascript, demo]
categories: [前端, javascript]
description:
---


今天把京东的分类栏给扒下来了，js部分是我自己写的，我不知道除了onmouseover和onmouseout之外，是否还有其他的方法，等以后遇到，会再来补充。


```html
<div id="category-2014" class="dropdown">
    <div class="dt">
        <a href="#" target="_blank">全部商品分类</a>
    </div>
    <div class="dd">
        <div class="dd-inner">
            <div class="item fore1" data-index="1">
                <h3>
                    <a href="#" target="_blank">家用电器</a>
                </h3>
                <i>></i>
            </div>
            <div class="item fore2" data-index="2">
                    <h3><a target="_blank" href="//shouji.jd.com/">手机</a>、<a target="_blank" href="//shuma.jd.com/">数码</a>、<a target="_blank" href="//mobile.jd.com/">京东通信</a></h3>
                    <i>></i>
                </div>
                <div class="item fore3" data-index="3">
                    <h3><a target="_blank" href="//diannao.jd.com/">电脑、办公</a></h3>
                    <i>></i>
                </div>
                <div class="item fore4" data-index="4">
                    <h3>
                      <a target="_blank" href="#">家居</a>、
                      <a target="_blank" href="#">家具</a>、
                      <a target="_blank" href="#">家装</a>、
                      <a target="_blank" href="#">厨具</a></h3>
                    <i>></i>
                </div>
                <div class="item fore5" data-index="5">
                    <h3>
                      <a target="_blank" href="#">男装</a>、
                      <a target="_blank" href="#">女装</a>、
                      <a target="_blank" href="#">童装</a>、
                      <a target="_blank" href="#">内衣</a>
                      </h3>
                    <i>></i>
                </div>
                <div class="item fore6" data-index="6">
                    <h3>
                      <a target="_blank" href="#">个护化妆</a>、
                      <a target="_blank" href="#">清洁用品</a>、
                      <a target="_blank" href="#">宠物</a>
                      </h3>
                    <i>></i>
                </div>
                <div class="item fore7" data-index="7">
                    <h3>
                      <a target="_blank" href="#">鞋靴</a>、
                      <a target="_blank" href="#">箱包</a>、
                      <a target="_blank" href="#">珠宝</a>、
                      <a target="_blank" href="#">奢侈品</a>
                      </h3>
                    <i>></i>
                </div>
                <div class="item fore8" data-index="8">
                    <h3>
                      <a target="_blank" href="#">运动户外</a>、
                      <a target="_blank" href="#">钟表</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore9" data-index="9">
                    <h3>
                      <a target="_blank" href="#">汽车</a>、
                      <a target="_blank" href="#">汽车用品</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore10" data-index="10">
                    <h3>
                      <a target="_blank" href="#">母婴</a>、
                      <a target="_blank" href="#">玩具乐器</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore11" data-index="11">
                    <h3>
                      <a target="_blank" href="#">食品</a>、
                      <a target="_blank" href="#">酒类</a>、
                      <a target="_blank" href="#">生鲜</a>、
                      <a target="_blank" href="#">特产</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore12" data-index="12">
                    <h3>
                      <a target="_blank" href="#">医药保健</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore13" data-index="13">
                    <h3>
                      <a target="_blank" href="#">图书</a>、
                      <a target="_blank" href="#">音像</a>、
                      <a target="_blank" href="#">电子书</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore14" data-index="14">
                    <h3>
                      <a target="_blank" href="#">彩票</a>、
                      <a target="_blank" href="#">旅行</a>、
                      <a target="_blank" href="#">充值</a>、
                      <a target="_blank" href="#">票务</a>
                    </h3>
                    <i>></i>
                </div>
                <div class="item fore15" data-index="15">
                    <h3>
                      <a target="_blank" href="#">理财</a>、
                      <a target="_blank" href="#">众筹</a>、
                      <a target="_blank" href="#">白条</a>、
                      <a target="_blank" href="#">保险</a>
                    </h3>
                    <i>></i>
                </div>

            <div class="dropdown-layer">
                <div class="item-sub" id="category-item-1">
                    <img src="img/pic01.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/pic02.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/pic03.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/1.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/2.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/3.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/4.jpg">
                </div>

                <div class="item-sub" id="category-item-2">
                    <img src="img/banner.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/5.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/6.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/7.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/pic02.jpg">
                </div>

                <div class="item-sub" id="category-item-2">
                    <img src="img/2.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/1.jpg">
                </div>
                <div class="item-sub" id="category-item-2">
                    <img src="img/4.jpg">
                </div>
            </div>
        </div>
    </div>
</div>
```

```css
* {
    margin: 0;
    padding: 0;
    font-family: 'Microsoft Yahei','黑体';
    -webkit-font-smoothing: antialiased;
}
a {
    color: #fff;
    text-decoration: none;
}
.dt a {
    display: block;
width: 190px;
height: 44px;
padding: 0 10px;
background: #B1191A;
font: 600 15px/44px "microsoft yahei";
color: #fff;
}
.dd {
    width: 210px;
    height: 466px;
background: #c81623;
margin-top: 2px;
}
.dd .dd-inner .item {
    border-left: 1px solid #b61d1d;
position: relative;
z-index: 1;
height: 31px;
color: #fff;
}
.dd .dd-inner .item h3 {
position: absolute;
z-index: 2;
height: 31px;
padding: 0 10px;
line-height: 31px;
font-family: "microsoft yahei";
font-size: 14px;
font-weight: 400;
}
.dd .dd-inner .item i {
display: block;
position: absolute;
z-index: 1;
top: 9px;
right: 14px;
width: 4px;
height: 14px;
font: 400 9px/14px consolas;
}

.dd .dropdown-layer{
    display: none;
position: absolute;
left: 209px;
top: 45px;
width: 779px;
height: 465px;
background: #f7f7f7;
border: 1px solid #b61d1d;
overflow: hidden;
}

.dd .dropdown-layer .item-sub {
    display: none;
}
.dd .dropdown-layer .item-sub,
.dd .dropdown-layer .item-sub img {
    width: 100%;
    height: 100%;
}

.dd .dd-inner .item-hover {
background-color: #fff;
color: #B61D1D;
font-weight: bolder;
}
.dd .dd-inner .item-hover a {
color: #B61D1D;
}
.dd .dd-inner .item-hover i {
display: none;
}
.dd .dd-inner .item-hover ~ .dropdown-layer {
display: block;
}
.dd .dropdown-layer .hover {
    display: block;
}
```


```js
var item = document.querySelectorAll('.item');
var category_item = document.querySelectorAll('.item-sub');
var dropdown_layer = document.querySelector('.dropdown-layer');

for(var i=0;i<item.length;i++) {
    item[i].index = i;
    item[i].onmouseover = function(event) {
        // 去除其他item的hover样式
        for(var j=0;j<item.length;j++) {
            item[j].className = "item";
        }

        // 为当前item添加hover样式
        this.className += ' item-hover';

        // 去除其他category_item的hover样式
        for(j=0;j<category_item.length;j++) {
            category_item[j].className = "item-sub";
        }

        // 为当前category_item添加hover样式
        category_item[this.index].className += ' hover';

        category_item[this.index].c_index = this.index;

        // 鼠标离开时去除样式
        category_item[this.index].onmouseout = function() {
             item[this.c_index].className = 'item';
             this.className = 'item-sub';
        }
    }
}
```




