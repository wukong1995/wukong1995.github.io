---
title: jQuery部分内容
date: 2016-04-17 19:54:29
tags: [javascript]
categories: [前端, javascript]
description:
---


#### 图片放大镜插件——jqzoom
在调用jqzoom图片放大镜插件时，需要准备一大一小两张一样的图片，在页面中显示小图片，当鼠标在小图片中移动时，调用该插件的jqzoom()方法，显示与小图片相同的大图片区域，从而实现放大镜的效果，调用格式如下：
$(linkimage).jqzoom({options})
其中linkimage参数为包含图片的<a>元素名称，options为插件方法的配置对象。

#### cookie插件——cookie
使用cookie插件后，可以很方便地通过cookie对象保存、读取、删除用户的信息，还能通过cookie插件保存用户的浏览记录，它的调用格式为：
保存：$.cookie(key，value)；读取：$.cookie(key)，删除：$.cookie(key，null)
其中参数key为保存cookie对象的名称，value为名称对应的cookie值。

#### 搜索插件——autocomplete
搜索插件的功能是通过插件的autocomplete()方法与文本框相绑定，当文本框输入字符时，绑定后的插件将返回与字符相近的字符串提示选择，调用格式如下：

#### $(textbox).autocomplete(urlData,[options]);
其中，textbox参数为文本框元素名称，urlData为插件返回的相近字符串数据，可选项参数options为调用插件方法时的配置对象。

#### 右键菜单插件——contextmenu
右键菜单插件可以绑定页面中的任意元素，绑定后，选中元素，点击右键，便通过该插件弹出一个快捷菜单，点击菜单各项名称执行相应操作，调用代码如下：

#### $(selector).contextMenu(menuId,{options});
Selector参数为绑定插件的元素，meunId为快捷菜单元素，options为配置对象。

#### 自定义对象级插件——lifocuscolor插件
自定义的lifocuscolor插件可以在<ul>元素中，鼠标在表项<li>元素移动时，自定义其获取焦点时的背景色，即定义<li>元素选中时的背景色，调用格式为：
`$(Id).focusColor(color)`
其中，参数Id表示<ul>元素的Id号，color表示<li>元素选中时的背景色。

#### 自定义类级别插件—— twoaddresult
通过调用自定义插件twoaddresult中的不同方法，可以实现对两个数值进行相加和相减的运算，导入插件后，调用格式分别为：
$.addNum(p1,p2) 和 $.subNum(p1,p2)
上述调用格式分别为计算两数值相加和相减的结果，p1和p2为任意数值。

#### 拖曳插件——draggable
拖曳插件draggable的功能是拖动被绑定的元素，当这个jQuery UI插件与元素绑定后，可以通过调用draggable()方法，实现各种拖曳元素的效果，调用格式如下：
$(selector). draggable({options})
options参数为方法调用时的配置对象，根据该对象可以设置各种拖曳效果，如“containment”属性指定拖曳区域，“axis”属性设置拖曳时的坐标方向。

#### 放置插件——droppable
除使用draggable插件拖曳任意元素外，还可以调用droppable UI插件将拖曳后的任意元素放置在指定区域中，类似购物车效果，调用格式如下：
$(selector).droppable({options})
selector参数为接收拖曳元素，options为方法的配置对象，在对象中，drop函数表示当被接收的拖曳元素完全进入接收元素的容器时，触发该函数的调用。

#### 拖曳排序插件——sortable
拖曳排序插件的功能是将序列元素（例如<option>、<li>）按任意位置进行拖曳从而形成一个新的元素序列，实现拖曳排序的功能，它的调用格式为：
$(selector).sortable({options});
selector参数为进行拖曳排序的元素，options为调用方法时的配置对象，
面板折叠插件——accordion
面板折叠插件可以实现页面中指定区域类似“手风琴”的折叠效果，即点击标题时展开内容，再点另一标题时，关闭已展开的内容，调用格式如下：
$(selector).accordion({options});
其中，参数selector为整个面板元素，options参数为方法对应的配置对象。

#### 选项卡插件——tabs
使用选项卡插件可以将<ul>中的<li>选项定义为选项标题，在标题中，再使用<a>元素的“href”属性设置选项标题对应的内容，它的调用格式如下：
$(selector).tabs({options});
selector参数为选项卡整体外围元素，该元素包含选项卡标题与内容，options参数为tabs()方法的配置对象，通过该对象还能以ajax方式加载选项卡的内容。

#### 对话框插件——dialog
对话框插件可以用动画的效果弹出多种类型的对话框，实现JavaScript代码中alert()和confirm()函数的功能，它的调用格式为：
$(selector).dialog({options});
selector参数为显示弹出对话框的元素，通常为<div>，options参数为方法的配置对象，在对象中可以设置对话框类型、“确定”、“取消”按钮执行的代码等。

#### 菜单工具插件——menu
菜单工具插件可以通过<ul>创建多级内联或弹出式菜单，支持通过键盘方向键控制菜单滑动，允许为菜单的各个选项添加图标，调用格式如下：
$(selector).menu({options});
selector参数为菜单列表中最外层<ul>元素，options为menu()方法的配置对象。

#### 微调按钮插件——spinner
微调按钮插件不仅能在文本框中直接输入数值，还可以通过点击输入框右侧的上下按钮修改输入框的值，还支持键盘的上下方向键改变输入值，调用格式如下：
$(selector).spinner({options});
selector参数为文本输入框元素，可选项options参数为spinner()方法的配置对象，在该对象中，可以设置输入的最大、最小值，获取改变值和设置对应事件。

#### 工具提示插件——tooltip
工具提示插件可以定制元素的提示外观，提示内容支持变量、Ajax远程获取，还可以自定义提示内容显示的位置，它的调用格式如下：
$(selector).tooltip({options});
其中selector为需要显示提示信息的元素，可选项参数options为tooltip()方法的配置对象，在该对象中，可以设置提示信息的弹出、隐藏时的效果和所在位置。

#### 获取浏览器的名称与版本信息
在jQuery中，通过$.browser对象可以获取浏览器的名称和版本信息，如$.browser.chrome为true，表示当前为Chrome浏览器，$.browser.mozilla为true，表示当前为火狐浏览器，还可以通过$.browser.version方式获取浏览器版本信息。

#### 检测浏览器是否属于W3C盒子模型
浏览器的盒子模型分为两类，一类为标准的w3c盒子模型，另一类为IE盒子模型，两者区别为在Width和Height这两个属性值中是否包含padding和border的值，w3c盒子模型不包含，IE盒子模型则包含，而在jQuery 中，可以通过$.support.boxModel对象返回的值，检测浏览器是否属于标准的w3c盒子模型。
检测对象是否为空

#### 在jQuery中，可以调用名为$.isEmptyObject的工具函数，检测一个对象的内容是否为空，如果为空，则该函数返回true，否则，返回false值，调用格式如下：
$.isEmptyObject(obj);
其中，参数obj表示需要检测的对象名称。

#### 检测对象是否为原始对象
调用名为$.isPlainObject的工具函数，能检测对象是否为通过{}或new Object()关键字创建的原始对象，如果是，返回true，否则，返回false值，调用格式为：
$.isPlainObject (obj);
其中，参数obj表示需要检测的对象名称。

#### 检测两个节点的包含关系
调用名为$.contains的工具函数，能检测在一个DOM节点中是否包含另外一个DOM节点，如果包含，返回true，否则，返回false值，调用格式为：
$.contains (container, contained);
参数container表示一个DOM对象节点元素，用于包含其他节点的容器，contained是另一个DOM对象节点元素，用于被其他容器所包含。

#### 字符串操作函数
调用名为$.trim的工具函数，能删除字符串中左右两边的空格符，但该函数不能删除字符串中间的空格，调用格式为：
$.trim (str);
参数str表示需要删除左右两边空格符的字符串。

#### URL操作函数
调用名为$. param的工具函数，能使对象或数组按照key/value格式进行序列化编码，该编码后的值常用于向服务端发送URL请求，调用格式为：
$. param (obj);
参数obj表示需要进行序列化的对象，该对象也可以是一个数组，整个函数返回一个经过序列化编码后的字符串
使用$.extend()扩展工具函数
调用名为$. extend的工具函数，可以对原有的工具函数进行扩展，自定义类级别的jQuery插件，调用格式为：
$. extend ({options});
参数options表示自定义插件的函数内容。
使用$.extend()扩展Object对象
除使用$.extend扩展工具函数外，还可以扩展原有的Object对象，在扩展对象时，两个对象将进行合并，当存在相同属性名时，后者将覆盖前者，调用格式为：
$. extend (obj1,obj2,…objN);
参数obj1至objN表示需要合并的各个原有对象。


