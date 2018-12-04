---
  title: CSS3实现二级菜单
  date: 2016-06-08 16:20:40
  tags: [css]
  categories:
    - [css]
    - [前端]
  description:
---


今天看了一个demo，原demo虽说是CSS3实现，但其中也使用了js，再一个里面的代码有些地方，我觉得还可以优化 ，所以自己就尝试着用css3实现一下

我想说: 不论写一个什么样的demo，z-index的值都最好不要超过2
下面是源码：

```html
<div class="sidebar">
    <h1><i class="fa fa-bars push"></i>Animated <span class="color">Menu</span></h1>
    <ul>
        <li><a href="#"><i class="fa fa-dashboard push"></i>Dashboard<i class="fa fa-angle-right"></i></a><span class="hover"></span>
        </li>
        <li><a href="#"><i class="fa fa-user push"></i>Users<i class="fa fa-angle-right"></i></a><span class="hover"></span>
            <ul class="sub-menu">
                <li><a href="#">Add User<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
                <li><a href="#">Manage Users<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
            </ul>
        </li>
        <li><a href="#"><i class="fa fa-cog push"></i>Settings<i class="fa fa-angle-right"></i></a><span class="hover"></span>
            <ul class="sub-menu">
                <li><a href="#">Dashboard Settings<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
                <li><a href="#">Profile Settings<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
                <li><a href="#">Manage Menu<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
                <li><a href="#">User Profiles<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
            </ul>
        </li>
        <li><a href="#"><i class="fa fa-picture-o push"></i>appearance<i class="fa fa-angle-right"></i></a><span class="hover"></span>
            <ul class="sub-menu">
                <li><a href="#">Change Theme<i class="fa fa-angle-right"></i></a><span class="hover"></span>
                </li>
                <li><a href="#">Theme Options<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
            </ul>
        </li>
        <li><a href="#"><i class="fa fa-file push"></i>Information<i class="fa fa-angle-right"></i></a><span class="hover"></span>
            <ul class="sub-menu">
                <li><a href="#">Latest News<i class="fa fa-angle-right"></i></a><span class="hover"></span>
                </li>
                <li><a href="#">Recent Articles<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
            </ul>
        </li>
        <li><a href="#"><i class="fa fa-plane push"></i>Contact<i class="fa fa-angle-right"></i></a><span class="hover"></span></li>
    </ul>
</div>
```

```css

* {
  padding: 0;
  margin: 0;
  font-family: "microsoft yahei"
}
a {
  color: #000;
  text-decoration: none;
}
ul {
  list-style: none
}

li {
  margin-right: 15px;
}
html,body {
  background-color: #808080;
  height: 100%;
}
.sidebar {
  background: url(img/menu_pattern_1.png);
  width: 200px;
  height: 100%;
  border-right: 10px solid #d00355;
  color: #fff;
  padding-left: 40px;
}
.sidebar h1 {
  font-size: 16px;
  height: 80px;
  line-height: 80px;
}
.sidebar h1 .color {
  color: #d00355;
}
.sidebar ul li {
  padding: 8px;
}
.sidebar ul li a {
  color: #6B6363;
  position: relative;
  z-index: 1;
}
.sidebar ul li a .fa-angle-right {
  position: absolute;
  left: 160px;
  top: 4px;
}
.sidebar ul li .hover {
  display: block;
  position: absolute;
  width: 0;
  height: 37px;
  margin-top: -30px;
  margin-left: -8px;
  z-index: 0;
  background-color: #d00355;
  opacity: 0;
  transition: all .5s .1s;
}
.sidebar ul li .sub-menu {
  display: block;
  background: url(img/menu_pattern_1.png);
  position: absolute;
  margin-left: 192px;
  margin-top: -31px;
  width: 200px;
  opacity: 0;
}
.sidebar > ul > li:hover ul {
  opacity: 1;
}
.sidebar ul li:hover .hover {
  width: 200px;
  opacity: 1;
}
.sidebar > ul > li:hover a{
  color: #fff;
}
```

1、在li标签中有两个元素，a标签和span标签，对于span标签，我使用了绝对定位，当然这个绝对定位和平常使用的绝对定位元素是不一样的，因为在li标签上没有相对定位这个属性，这个小技巧是来源于一个博客。而a标签中的class为fa-angle-right使用了绝对定位，与span标签是一种想法，a标签上我也不想给它加上相对定位这个包袱。可是，问题出现了，无论我怎样调整a标签和span标签的z-index，span标签都会把a标签给盖住，去网上搜了一下解决办法，于是给a加上了绝对定位这个属性，于是可以达到我想要的效果了，所以我就把fa-angle-right的属性改为了平常使用的绝对定位。。。真是不甘心呀。。
2、对于class为sub-menu的ul中的span，当我将鼠标滑过li标签时，li标签会出现span的宽度由0到200的效果，但是class为sub-menu的ul中的span却不会出现类似的效果，在测试时，如果将sub-menu的display属性设置为block，那麽滑过相应的li时，sub-menu中的span会出现想要的效果，当时很是着急，想来想去，问题可能出现在display这个属性上，虽然找不到具体的原因是什么，只能找对应的解决办法，有其他的属性，可以让一个元素从无变有吗？有opcity属性，于是将display的变化，改为了opcity的变化，想要的效果就出现了，用display，出现不了想要的效果，得原因是什么？需要去查查。。。

PS：css未做前缀的处理，所有的测试在最新版本的谷歌浏览器下进行

补充：
修改一下二级菜单中的字体大小，
text-transform 属性控制文本的大小写，值none/capitalize/uppercase/lowercase/inherit


