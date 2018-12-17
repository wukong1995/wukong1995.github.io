---
title: css3中的部分属性
date: 2016-05-30 20:42:12
tags: [css]
categories:
  - [css]
  - [前端]
description:
---

#### box-shadow
1、阴影box-shadow:x轴偏移量 y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式]
注意：inset（内阴影） 可以写在参数的第一个或最后一个，其它位置是无效的实例
正值表示在对象的底部，负值表示在对象的顶部。
```css
.boxshadow-outset{
    width:100px;height:100px;
    box-shadow:4px 4px 6px blue,4px 4px 6px red inset; 
}
.boxshadow-inset{
    width:100px;
    height:100px;
    box-shadow:4px 4px 6px #666 inset; 
}
```

##### 阴影模糊半径与阴影扩展半径的区别
阴影模糊半径：此参数可选，其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
阴影扩展半径：此参数可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；

##### X轴偏移量和Y轴偏移量值可以设置为负数
1、为边框应用图片 border-image
   顾名思义就是为边框应用背景图片，它和我们常用的background属性比较相似
   background:url(xx.jpg) 10px 20px no-repeat;
   另有属性round、stretch。
   用法：border-image:url(borderimg.png) 70 stretch

2、颜色之RGBA：在RGB的基础上增加了控制alpha透明度的参数
   语法color：rgba(R,G,B,A)

3、渐变色彩
   CSS3 Gradient 分为线性渐变(linear)和径向渐变(radial)
   background-image:linear-gradient(to left, red, orange,yellow,green,blue,indigo,violet);

4、text-overflow 与 word-wrap
   text-overflow用来设置是否使用一个省略标记（...）标示对象内文本的溢出   属性clip（表示剪切）和ellipsis（表示显示省略标记）
   但是text-overflow只是用来说明文字溢出时用什么方式显示，要实现溢出时   产生省略号的效果，还须定义强制文本在一行内显示（white-space:nowrap）   及溢出内容为隐藏（overflow:hidden），只有这样才能实现溢出文本显示省   略号的效果，代码如下：
text-overflow:ellipsis; 
overflow:hidden; 
white-space:nowrap; 
同时，word-wrap也可以用来设置文本行为，当前行超过指定容器的边界时是否断开转行。属性：normal（表示控制连续文本换行）和break-word（表示内容将在边界内换行）

6、嵌入字体@font-face
@font-face能够加载服务器端的字体文件，让浏览器端可以显示用户电脑里没有安装的字体。@font-face {
    font-family : 字体名称;
    src : 字体文件在服务器上的相对或绝对路径;}

7、文本阴影text-shadow可以设置文本的阴影效果
   text-shadow: X-Offset Y-Offset（阴影的垂直偏移距离） blur（模糊程度） color（阴影的颜色）;

8、background-origin设置元素背景图片的原始起始位置。
   background-origin ： border-box | padding-box | content-box;
   参数分别表示背景图片是从边框，还是内边距（默认值），或者是内容区域开   始显示。需要注意的是，如果背景不是no-repeat，这个属性无效，它会从边框开始显示。

9、background-clip用来将背景图片做适当的裁剪以适应实际需要。
   background-clip ： border-box | padding-box | content-box | no-clip
   参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box。

10、background-size设置背景图片的大小，以长度值或百分比显示，还可以通过cover和contain来对图片进行伸缩。

#### background-size: auto | <长度值> | <百分比> | cover | contain
1、auto：默认值，不改变背景图片的原始高度和宽度；
2、<长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；
3、<百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；
4、cover：顾名思义为覆盖，即将背景图片等比缩放以填满整个容器；
5、contain：容纳，即将背景图片等比缩放至某一边紧贴容器边缘为止。
11、multiple backgrounds
多重背景，也就是CSS2里background的属性外加origin、clip和size组成的新background的多次叠加，缩写时为用逗号隔开的每组值；用分解写法时，如果有多个背景图片，而其他属性只有一个（例如background-repeat只有一个），表明所有背景图片应用该属性值。
background ： [background-color] | [background-image] | [background-position][/background-size] | [background-repeat] | [background-attachment] | [background-clip] | [background-origin],...
12、属性选择器
```html
<a href="xxx.pdf">我链接的是PDF文件</a>
<a href="#" class="icon">我类名是icon</a>
<a href="#" title="我的title是more">我的title是more</a>
```

```css
a[class^=icon]{
  background: green;
  color:#fff;
}
a[href$=pdf]{
  background: orange;
  color: #fff;
}
a[title*=more]{
  background: blue;
  color: #fff;
}
```

13、结构性伪类选择器--root：:root选择器，从字面上我们就可以很清楚的理解是根选择器，他的意思就是匹配元素E所在文档的根元素。在HTML文档中，根元素始终是<html>。“:root”选择器等同于<html>元素，简单点说：
:root{background:orange}==html {background:orange;}
14、结构性伪类选择器—not
:not选择器称为否定选择器，和jQuery中的:not选择器一模一样，可以选择除某个元素之外的所有元素。就拿form元素来说，比如说你想给表单中除submit按钮之外的input元素添加红色边框，CSS代码可以写成：
```css
form {
  width: 200px;
  margin: 20px auto;}
div {
  margin-bottom: 20px;
}
input:not([type="submit"]){
  border:1px solid red;
}
```
15、结构性伪类选择器—empty
:empty选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格。
比如说，你的文档中有三个段落p元素，你想把没有任何内容的P元素隐藏起来。我们就可以使用“:empty”选择器来控制。

```html
<p>我是一个段落</p>
<p> </p>
<p></p>
```

```css
p{
 background: orange;
 min-height: 30px;
}
p:empty {
  display: none;
}
```
16、结构性伪类选择器—target
:target选择器称为目标选择器，用来匹配文档(页面)的url的某个标志符的目标元素。我们先来上个例子，然后再做分析。
示例展示
点击链接显示隐藏的段落。
```html
<h2><a href="#brand">Brand</a></h2>
<div class="menuSection" id="brand">
    content for Brand
</div>
```

```css
.menuSection{
  display: none;
}
:target{/*这里的:target就是指id="brand"的div对象*/
  display:block;
}
```
17、结构性伪类选择器—first-child
“:first-child”选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个子元素，记住是子元素，而不是后代元素。

18、结构性伪类选择器—last-child
“:last-child”选择器与“:first-child”选择器作用类似，不同的是“:last-child”选择器选择的是元素的最后一个子元素。

19、结构性伪类选择器—nth-child(n)
“:nth-child(n)”选择器用来定位某个父元素的一个或多个特定的子元素。其中“n”是其参数，而且可以是整数值(1,2,3,4)，也可以是表达式(2n+1、-n+5)和关键词(odd、even)，但参数n的起始值始终是1，而不是0。也就是说，参数n的值为0时，选择器将选择不到任何匹配的元素。

20、结构性伪类选择器—nth-last-child(n)
“:nth-last-child(n)”选择器和前面的“:nth-child(n)”选择器非常的相似，只是这里多了一个“last”，所起的作用和“:nth-child(n)”选择器有所区别，从某父元素的最后一个子元素开始计算，来选择特定的元素。

21、first-of-type选择器
“:first-of-type”选择器类似于“:first-child”选择器，不同之处就是指定了元素的类型,其主要用来定位一个父元素下的某个类型的第一个子元素。

22、nth-of-type(n)选择器
“:nth-of-type(n)”选择器和“:nth-child(n)”选择器非常类似，不同的是它只计算父元素中指定的某种类型的子元素。当某个元素中的子元素不单单是同一种类型的子元素时，使用“:nth-of-type(n)”选择器来定位于父元素中某种类型的子元素是非常方便和有用的。在“:nth-of-type(n)”选择器中的“n”和“:nth-child(n)”选择器中的“n”参数也一样，可以是具体的整数，也可以是表达式，还可以是关键词
n的参数也可以为odd 和 even 是可用于匹��下标是奇数或偶数的子元素的关键词

23、last-of-type选择器
“:last-of-type”选择器和“:first-of-type”选择器功能是一样的，不同的是他选择是父元素下的某个类型的最后一个子元素

24、nth-last-of-type(n)选择器
“:nth-last-of-type(n)”选择器和“:nth-of-type(n)”选择器是一样的，选择父元素中指定的某种子元素类型，但它的起始方向是从最后一个子元素开始，而且它的使用方法类似于上节中介绍的“:nth-last-child(n)”选择器一样。

25、only-child选择器
“:only-child”选择器选择的是父元素中只有一个子元素，而且只有唯一的一个子元素。也就是说，匹配的元素的父元素中仅有一个子元素，而且是一个唯一的子元素。

26、only-of-type选择器
“:only-of-type”选择器用来选择一个元素是它的父元素的唯一一个相同类型的子元素。这样说或许不太好理解，换一种说法。“:only-of-type”是表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的，使用“:only-of-type”选择器就可以选中这个元素中的唯一一个类型子元素。

27、:enabled选择器
在Web的表单中，有些表单元素有可用（“:enabled”）和不可用（“:disabled”）状态，比如输入框，密码框，复选框等。在默认情况之下，这些表单元素都处在可用状态。那么我们可以通过伪选择器“:enabled”对这些表单元素设置样式。

28、:disabled选择器
“:disabled”选择器刚好与“:enabled”选择器相反，用来选择不可用表单元素。要正常使用“:disabled”选择器，需要在表单元素的HTML中设置“disabled”属性。

29、:checked选择器
在表单元素中，单选按钮和复选按钮都具有选中和未选中状态。（大家都知道，要覆写这两个按钮默认样式比较困难）。在CSS3中，我们可以通过状态选择器“:checked”配合其他标签实现自定义样式。而“:checked”表示的是选中状态。

30、::selection选择器
“::selection”伪元素是用来匹配突出显示的文本(用鼠标选择文本时的文本)。浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的，效果如下图所示：

31、:read-only选择器
“:read-only”伪类选择器用来指定处于只读状态元素的样式。简单点理解就是，元素中设置了“readonly=’readonly’

32、:read-write选择器
“:read-write”选择器刚好与“:read-only”选择器相反，主要用来指定当元素处于非只读状态时的样式。

33、::before和::after
::before和::after这两个主要用来给元素的前面或后面插入内容，这两个常和"content"配合使用，使用的场景最多的就是清除浮动。

34、变形--旋转 rotate()
旋转rotate()函数通过指定的角度参数使元素相对原点进行旋转。它主要在二维空间内进行操作，设置一个角度值，用来指定旋转的幅度。如果这个值为正值，元素相对原点中心顺时针旋转；如果这个值为负值，元素相对原点中心逆时针旋转

35、变形--旋转 rotate()
旋转rotate()函数通过指定的角度参数使元素相对原点进行旋转。它主要在二维空间内进行操作，设置一个角度值，用来指定旋转的幅度。如果这个值为正值，元素相对原点中心顺时针旋转；如果这个值为负值，元素相对原点中心逆时针旋转。

36、变形--扭曲 skew()
扭曲skew()函数能够让元素倾斜显示。它可以将一个对象以其中心位置围绕着X轴和Y轴按照一定的角度倾斜。这与rotate()函数的旋转不同，rotate()函数只是旋转，而不会改变元素的形状。skew()函数不会旋转，而只会改变元素的形状。
1、skew(x,y)使元素在水平和垂直方向同时扭曲
2、skewX(x)仅使元素在水平方向扭曲变形（X轴扭曲变形）
3、skewY(y)仅使元素在垂直方向扭曲变形（Y轴扭曲变形）

37、变形--缩放 scale()缩放 scale()函数 让元素根据中心原点对对象进行缩放
1、 scale(X,Y)使元素水平方向和垂直方向同时缩放。若只有一个参数，则xy同时缩放
2、scaleX(x)元素仅水平方向缩放（X轴缩放）
3、scaleY(y)元素仅垂直方向缩放（Y轴缩放）

38、变形--位移 translate()
translate()函数可以将元素向指定的方向移动，类似于position中的relative。或以简单的理解为，使用translate()函数，可以把元素从原来的位置移动，而不影响在X、Y轴上的任何Web组件。
1、translate(x,y)水平方向和垂直方向同时移动
2、translateX(x)仅水平方向移动（X轴移动）
3、translateY(Y)仅垂直方向移动（Y轴移动）

39、变形--矩阵 matrix()
matrix() 是一个含六个值的(a,b,c,d,e,f)变换矩阵，用来指定一个2D变换，相当于直接应用一个[a b c d e f]变换矩阵。就是基于水平方向（X轴）和垂直方向（Y轴）重新定位元素,此属性值使用涉及到数学中的矩阵，我在这里只是简单的说一下CSS3中的transform有这么一个属性值，如果需要深入了解，需要对数学矩阵有一定的知识。

40、变形--原点 transform-origin
在没有重置transform-origin改变元素原点位置的情况下，CSS变形进行的旋转、位移、缩放，扭曲等操作都是以元素自己中心位置进行变形。但很多时候，我们可以通过transform-origin来对元素进行原点位置改变，使元素原点不在元素的中心位置，以达到需要的原点位置。
对于x轴的调整：left|center|right
对于y轴的调整：top|center|bottom
对于z轴的调整：length px

41、动画--过渡属性 transition-property
早期在Web中要实现动画效果，都是依赖于JavaScript或Flash来完成。但在CSS3中新增加了一个新的模块transition，它可以通过一些简单的CSS事件来触发元素的外观变化，让效果显得更加细腻。简单点说，就是通过鼠标的单击、获得焦点，被点击或对元素任何改变中触发，并平滑地以动画效果改变CSS的属性值。
在CSS中创建简单的过渡效果可以从以下几个步骤来实现：
第一，在默认样式中声明元素的初始状态样式；
第二，声明过渡元素最终状态样式，比如悬浮状态；
第三，在默认样式中通过添加过渡函数，添加一些不同的样式。
CSS3的过度transition:属性名称|过渡所用时间|过渡模式:，主要包括以下几个子属性：
transition-property:none/all/某一属性名称指定过渡或动态模拟的CSS属性
transition-duration:指定完成过渡所需的时间
transition-duration属性主要用来设置一个属性过渡到另一个属性所需的时间，也就是从旧属性过渡到新属性花费的时间长度，俗称持续时间。
transition-timing-function:指定过渡函数，
transition-timing-function：ease(缓慢开始，缓慢结束)|linear(匀速)|ease-in(缓慢开始)|ease-out(缓慢结束)|ease-in-out(缓慢开始，缓慢结束)。默认ease
transition-delay:指定开始出现的延迟时间，transition-delay属性和transition-duration属性极其类似，不同的是transition-duration是用来设置过渡动画的持续时间，而transition-delay主要用来指定一个动画开始执行的时间，也就是说当改变元素属性值后多长时间开始执行。
有时我们想改变两个或者多个css属性的transition效果时，只要把几个transition的声明串在一起，用逗号（“，”）隔开，然后各自可以有各自不同的延续时间和其时间的速率变换方式。但需要值得注意的一点：第一个时间的值为 transition-duration，第二个为transition-delay。

42、Keyframes介绍
Keyframes被称为关键帧，其类似于Flash中的关键帧。在CSS3中其主要以“@keyframes”开头，后面紧跟着是动画名称加上一对花括号“{…}”，括号中就是一些不同时间段样式规则。
@keyframes changecolor{
  0%{
   background: red;
  }
  100%{
    background: green;
  }
}
在一个“@keyframes”中的样式规则可以由多个百分比构成的，如在“0%”到“100%”之间创建更多个百分比，分别给每个百分比中给需要有动画效果的元素加上不同的样式，从而达到一种在不断变化的效果。
经验与技巧：在@keyframes中定义动画名称时，其中0%和100%还可以使用关键词from和to来代表，其中0%对应的是from，100%对应的是to。
aniamtion:name|duration|timing-function|delay|iteration-count|direction|play-state|fill-mode

43、调用动画
animation-name属性主要是用来调用 @keyframes 定义好的动画。需要特别注意: animation-name 调用的动画名需要和“@keyframes”定义的动画名称完全一致（区分大小写），如果不一致将不具有任何动画效果。
语法：animation-name: none | IDENT[,none|DENT]*;

44、设置动画播放时间
animation-duration主要用来设置CSS3动画播放时间，其使用方法和transition-duration类似，是用来指定元素播放动画所持续的时间长，也就是完成从0%到100%一次动画所需时间。单位：S秒
语法规则
animation-duration: <time>[,<time>]*
取值<time>为数值，单位为秒，其默认值为“0”，这意味着动画周期为“0”，也就是没有动画效果（如果值为负值会被视为“0”）。

45、设置动画播放方式
animation-timing-function属性主要用来设置动画播放方式。主要让元素根据时间的推进来改变属性值的变换速率，简单点说就是动画的播放方式。
语法规则：
animation-timing-function:ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [, ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)]*
它和transition中的transition-timing-function一样，具有以下几种变换方式：ease,ease-in,ease-in-out,ease-out,linear和cubic-bezier。对应功如下：

46、设置动画开始播放的时间
animation-delay属性用来定义动画开始播放的时间，用来触发动画播放的时间点。和transition-delay属性一样，用于定义在浏览器开始执行动画之前等待的时间。
语法规则：
animation-delay:<time>[,<time>]*

47、设置动画播放次数
animation-iteration-count属性主要用来定义动画的播放次数。
语法规则：
animation-iteration-count: infinite | <number> [, infinite | <number>]*
1、其值通常为整数，但也可以使用带有小数的数字，其默认值为1，这意味着动画将从开始到结束只播放一次。
2、如果取值为infinite，动画将会无限次的播放
注意：Chrome或Safari浏览器，需要加入-webkit-前缀！

48、设置动画播放方向（需加前缀）
animation-direction属性主要用来设置动画播放方向，其语法规则如下：
animation-direction:normal | alternate [, normal | alternate]*
其主要有两个值：normal、alternate
1、normal是默认值，如果设置为normal时，动画的每次循环都是向前播放；
2、另一个值是alternate，他的作用是，动画播放在第偶数次向前播放，第奇数次向反方向播放。

49、设置动画的播放状态
animation-play-state属性主要用来控制元素动画的播放状态。
参数：其主要有两个值：running和paused。
其中running是其默认值，主要作用就是类似于音乐播放器一样，可以通过paused将正在播放的动画停下来，也可以通过running将暂停的动画重新播放，这里的重新播放不一定是从元素动画的开始播放，而是从暂停的那个位置开始播放。另外如果暂停了动画的播放，元素的样式将回到最原始设置状态。

50、设置动画时间外属性
animation-fill-mode属性定义在动画开始之前和结束之后发生的操作。主要具有四个属性值：none、forwards、backwords和both。其四个属性值对应效果如下：
none：默认值，表示动画将按预期进行和结束，在动画完成其最后一帧时，动画会反转到初始帧
forwards：
表示动画在结束后继续应用最后的关键帧的位置
backwards：
会在向元素应用动画样式时迅速应用动画的初始帧
both：
元素动画同时具有forwards和backwards效果
在默认情况之下，动画不会影响它的关键帧之外的属性，使用animation-fill-mode属性可以修改动画的默认行为。简单的说就是告诉动画在第一关键帧上等待动画开始，或者在动画结束时停在最后一个关键帧上而不回到动画的第一帧上。或者同时具有这两个效果。

51、多列布局——Columns
为了能在Web页面中方便实现类似报纸、杂志那种多列排版的布局，W3C特意给CSS3增加了一个多列布局模块（CSS Multi Column Layout Module）。它主要应用在文本的多列布局方面，这种布局在报纸和杂志上都使用了几十年了，但要在Web页面上实现这样的效果还是有相当大的难度，庆幸的是，CSS3的多列布局可以轻松实现。接下来咱们一起学习多列布局相关的知识。
语法：columns：<column-width> || <column-count>

52、多列布局——column-width
column-width的使用和CSS中的width属性一样，不过不同的是，column-width属性在定义元素列宽的时候，既可以单独使用，也可以和多列属性中其他属性配合使用。其基本语法如下所示：column-width: auto | <length>

53、多列布局——column-count
column-count属性主要用来给元素指定想要的列数和允许的最大列数。其语法规则：column-count：auto | <integer>

54、列间距column-gap
column-gap主要用来设置列与列之间的间距，其语法规则如下：column-gap: normal || <length> 注意：normal默认值为1em，如果你的字号是px，则默认值为字体的大小

55、列表边框column-rule
column-rule主要是用来定义列与列之间的边框宽度、边框样式和边框颜色。简单点说，就有点类似于常用的border属性。但column-rule是不占用任何空间位置的，在列与列之间改变其宽度不会改变任何列的位置。
语法规则：column-rule:<column-rule-width>|<column-rule-style>|<column-rule-color>三个属性类似于border的属性

56、跨列设置column-span
column-span主要用来定义一个分列元素中的子元素能跨列多少。column-width、column-count等属性能让一元素分成多列，不管里面元素如何排放顺序，他们都是从左向右的放置内容，但有时我们需要基中一段内容或一个标题不进行分列，也就是横跨所有列，此时column-span就可以轻松实现，此属性的语法如下��
column-span: none | all  注意：none为默认值，表示不跨越任何列，all表示跨越所有列

57、盒模型在CSS3中新增加了box-sizing属性，能够事先定义盒模型的尺寸解析方式，其语法规则如下：
box-sizing: content-box | border-box | inherit
content-box：默认值，其让元素维持W3C的标准盒模型，也就是说元素的宽度和高度（width/height）等于元素边框宽度（border）加上元素内距（padding）加上元素内容宽度或高度（content width/ height），也就是element width/height = border + padding + content width / height
border-box：重新定义CSS2.1中盒模型组成的模式，让元素维持IE传统的盒模型（IE6以下版本和IE6-7怪异模式），也就是说元素的宽度或高度等于元素内容的宽度或高度。从上面盒模型介绍可知，这里的内容宽度或高度包含了元素的border、padding、内容的宽度或高度（此处的内容宽度或高度＝盒子的宽度或高度—边框—内距）。
inherit：使元素继承父元素的盒模型模式

58、伸缩布局（一）
CSS3引入了一种新的布局模式——Flexbox布局，即伸缩布局盒模型（Flexible Box），用来提供一个更加有效的方式制定、调整和分布一个容器里项目布局，即使它们的大小是未知或者动态的，这里简称为Flex。
Flexbox布局常用于设计比较复杂的页面，可以轻松的实现屏幕和浏览器窗口大小发生变化时保持元素的相对位置和大小不变，同时减少了依赖于浮动布局实现元素位置的定义以及重置元素的大小。
Flexbox布局在定义伸缩项目大小时伸缩容器会预留一些可用空间，让你可以调节伸缩项目的相对大小和位置。例如，你可以确保伸缩容器中的多余空间平均分配多个伸缩项目，当然，如果你的伸缩容器没有足够大的空间放置伸缩项目时，浏览器会根据一定的比例减少伸缩项目的大小，使其不溢出伸缩容器。综合而言，Flexbox布局功能主要具有以下几点：
第一，屏幕和浏览器窗口大小发生改变也可以灵活调整布局；
第二，可以指定伸缩项目沿着主轴或侧轴按比例分配额外空间（伸缩容器额外空间），从而调整伸缩项目的大小；
第三，可以指定伸缩项目沿着主轴或侧轴将伸缩容器额外空间，分配到伸缩项目之前、之后或之间；
第四，可以指定如何将垂直于元素布局轴的额外空间分布到该元素的周围；
第五，可以控制元素在页面上的布局方向；
第六，可以按照不同于文档对象模型（DOM）所指定排序方式对屏幕上的元素重新排序。也就是说可以在浏览器渲染中不按照文档流先后顺序重排伸缩项目顺序。
Flexbox规范版本众多，浏览器对此语法支持度也各有不同，接下来的内容以最新语法版本为例向大家展
1.创建一个flex容器
任何一个flexbox布局的第一步是需要创建一个flex容器。为此给元素设置display属性的值为flex。在Safari浏览器中，你依然需要添加前缀-webkit，
.flexcontainer{ display: -webkit-flex; display: flex; }
2.Flex项目显示
Flex项目是Flex容器的子元素。他们沿着主要轴和横轴定位。默认的是沿着水平轴排列一行。你可以通过flex-direction来改变主轴方向修改为column，其默认值是row。
4.Flex项目移动到顶部
如何将flex项目移动到顶部，取决于主轴的方向。如果它是垂直的方向通过align-items设置；如果它是水平的方向通过justify-content设置。
5.Flex项目移到左边
flex项目称动到左边或右边也取决于主轴的方向。如果flex-direction设置为row，设置justify-content控制方向；如果设置为column，设置align-items控制方向。-webkit-align-items: flex-start/flex-end/center;
8.Flex项目实现自动伸缩
您可以定义一个flex项目，如何相对于flex容器实现自动的伸缩。需要给每个flex项目设置flex属性设置需要伸缩的值。
.bigitem{ -webkit-flex:200; flex:200; }  .smallitem{ -webkit-flex:100; flex:100; }

59、Media Queries——媒体类型（一）
随着科学技术不断的向前发展，网页的浏览终端越来越多样化，用户可以通过：宽屏电视、台式电脑、笔记本电脑、平板电脑和智能手机来访问你的网站。尽管你无法保证一个网站在不同屏幕尺寸和不同设备上看起来完全一模一样，但至少要让你的Web页面能适配用户的终端，让他更好的呈现在你的用户面前。在本节中，将会学到如何使用CSS3中的Media Queries模块来让一个页面适应不同的终端（或屏幕尺寸），从而让你的页面让用户有一个更好的体验。
一、媒体类型
媒体类型（Media Type）在CSS2中是一个常见的属性，也是一个非常有用的属性，可以通过媒体类型对不同的设备指定不同的样式。见图片见素材/图片

60、responsive布局技巧
方法。你首先禁掉你页面中所有的样式（以及与样式相关的信息），在浏览器中打开，如果你的内容排列有序，方便阅读，那么你的这个结构不会差到哪里去。

61、自由缩放属性resize
resize属性主要是用来改变元素尺寸大小的，其主要目的是增强用户体验。但使用方法却是极其的简单，先从其语法入手。
resize: none | both | horizontal | vertical | inherit
none用户不能拖动元素尺寸大小，both用户可以拖动元素，同时可以修改元素的宽度和高度，horizontal用户可以拖放元素，仅可以修改元素的宽度，但不能修改元素的高度，vertical用户可以拖放元素，尽可以修改元素的高度，但不能修改元素的宽度，inherit继承父元素的属性值

62、CSS3外轮廓属性
外轮廓outline在页面中呈现的效果和边框border呈现的效果极其相似，但和元素边框border完全不同，外轮廓线不占用网页布局空间，不一定是矩形，外轮廓是属于一种动态样式，只有元素获取到焦点或者被激活时呈现。
outline属性早在CSS2中就出现了，主要是用来在元素周围绘制一条轮廓线，可以起到突出元素的作用。但是并未得到各主流浏览器的广泛支持，在CSS3中对outline作了一定的扩展，在以前的基础上增加新特性。outline属性的基本语法如下：
outline: ［outline-color］ || [outline-style] || [outline-width] || [outline-offset] || inherit
从语法中可以看出outline和border边框属性的使用方法极其类似。outline-color相当于border-color、outline-style相当于border-style，而outline-width相当于border-width，只不过CSS3给outline属性增加了一个outline-offset属性
outline-color：定义轮廓线的颜色，属性值为CSS中定义的颜色值。在实际应用中，可以将此参数省略，省略时此参数的默认值为黑色。
outline-style：定义轮廓线的样式，属性为CSS中定义线的样式。在实际应用中，可以将此参数省略，省略时此参数的默认值为none，省略后不对该轮廓线进行任何绘制。
outline-width：定义轮廓线的宽度，属性值可以为一个宽度值。在实际应用中，可以将此参数省略，省略时此参数的默认值为medium，表示绘制中等宽度的轮廓线。
outline-offset：定义轮廓边框的偏移位置的数值，此值可以取负数值。当此参数的值为正数值，表示轮廓边框向外偏离多少个像素；当此参数的值为负数值，表示轮廓边框向内偏移多少个像素。
inherit：元素继承父元素的outline效果。

63、CSS生成内容
在Web中插入内容，在CSS2.1时代依靠的是JavaScript来实现。但进入CSS3进代之后我们可以通过CSS3的伪类“:before”，“:after”和CSS3的伪元素“::before”、“::after”来实现，其关键是依靠CSS3中的“content”属性来实现。不过这个属性对于img和input元素不起作用。
content配合CSS的伪类或者伪元素，一般可以做以下四件事情：
none：不生成任何内容
attr：插入标签属性值
url：使用指定的绝对或相对地址插入一个外部资源（图像，声频，视频或浏览器支持的其他任何资源）
string：插入字符串
在CSS中有一种清除浮动的方法叫“clearfix”

64、CSS3 perspective 属性
定义3D元素距视图的距离，以像素计。该属性允许你改变3D元素查看3D元素的试图，当为元素定义perspective属性是，其子元素会获得透视效果，而不是元素本身。注释：perspective 属性只影响 3D 转换元素。
perspective: number|none;

65、CSS3 transform-style 属性
transform-style 属性规定如何在 3D 空间中呈现被嵌套的元素。
注释：该属性必须与 transform 属性一同使用。
transform-style: flat|preserve-3d;flat子元素将不保留3D位置，preserve-3d子元素将保留其3D位置

66、CSS3 transform 属性
transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜
translate系列单位是px， rotate系列单位是deg;translate和rotate顺序不同，效果也会不同
transform-origin属性定义变形的原点，默认是文件的中心。可以使用这个属性来改变变形的原点。

67、CSS vertical-align 属性
vertical-align 属性设置元素的垂直对齐方式。

68、CSS strong 属性
用于强调，和<em>标签一样，用于强调文本，但它强调的程度更强一下。通常使用加粗的字体来显示其中的内容

69、CSS display 属性
display属性规定元素应该生成的框的类型。这个属性用于定义建立布局时元素生成的显示框类型。对于 HTML 等文档类型，如果使用 display 不谨慎会很危险，因为可能违反 HTML 中已经定义的显示层次结构。对于 XML，由于 XML 没有内置的这种层次结构，所有 display 是绝对必要的。
70、CSS 属性
cursor:指示鼠标到达元素时的鼠标形态
71、font-smoothing属性
css3种用于设置字体的抗锯齿或者光滑度的属性
语法规则：font-smoothing:subpixel-antialiased(浏览器默认)|none(小像素文本)|antialiased(反锯齿)
72、如何隐藏一个元素，使其不可见
display:none;position:absolute;left:-999999px;visibility:hidden;opacity:0
opacity设置div元素的不透明级别。opacity: value(0.0完全透明-1.0完全不透明)|inherit(父类继承);
73、backface-visibility:定义当元素不面向屏幕是是否可见
backface-visibility:visible(背面是可见的)|hidden(背面是不可见的);
74、@font-face是css中的一个模块，主要用于将自己定义的web字体嵌入网页中
@font-face {
      font-family: <YourWebFontName>;
      src: <source> [<format>][,<source> [<format>]]*;
      [font-weight: <weight>];
      [font-style: <style>];
    }
取值说明
1、YourWebFontName:此值指的就是你自定义的字体名称，最好是使用你下载的默认字体，他将被引用到你的Web元素中的font-family。如“font-family:"YourWebFontName";”
2、source:此值指的是你自定义的字体的存放路径，可以是相对路径也可以是绝路径；
3、format：此值指的是你自定义的字体的格式，主要用来帮助浏览器识别，其值主要有以下几种类型：truetype,opentype,truetype-aat,embedded-opentype,avg等；
4、weight和style:这两个值大家一定很熟悉，weight定义字体是否为粗体，style主要定义字体样式，如斜体。
css shake:
<link type="text/css" href="csshake.css">
具体样式：
<div class="shake shake-hard"></div>
<div class="shake shake-slow"></div>
<div class="shake shake-little"></div>
<div class="shake shake-horizontal"></div>
<div class="shake shake.vertical"></div>
<div class="shake shake-rotate"></div>
<div class="shake shake-opacity"></div>
<div class="shake shake-crazy"></div>
75、box-sizing属性允许您以特定的方式定义匹配某个区域的特定元素。
box-sizing: content-box|border-box|inherit;
content-box:这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。
border-box:为元素设定的宽度和高度决定了元素的边框盒。就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。
inherit 规定应从父元素继承 box-sizing 属性的值。

