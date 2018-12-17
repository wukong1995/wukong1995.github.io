---
title: javaScript封装的常用函数（持续更新中）
date: 2016-04-24 11:43:10
tags: [javascript]
categories: [前端, javascript]
description:
---


整理了一些有用的javaScript片段
```js
// 2.通用函数
var g = function(id) {
	if (id.substr(0, 1) == '.') {
		return document.getElementsByClassName(id.substr(1));
	}
	return document.getElementById(id);
}

// getElementById
var $ = function(id) {
	return typeof id === "string" ? document.getElementById(id) : id;
}

// 为元素绑定事件
var addEvent = function(id, event, fn) {
	var el = $(id) || document;
	if (el.addEventListener) {
		el.addEventListener(event, fn, false);
	} else if (el.attachEvent) {
		el.attachEvent('on' + event, fn);
	} else {
		el['on' + event] = fn
	}
}

// 为元素绑定事件
var addEvent = function(el, event, fn) {
	if (el.addEventListener) {
		el.addEventListener(event, fn, false);
	} else if (el.attachEvent) {
		el.attachEvent('on' + event, fn);
	} else {
		el['on' + event] = fn
	}
}

// ajaxGet
var ajaxGet = function(url,callback) {
    var _xhr = null;
    if (window.XMLHttpRequest) {
        _xhr = new XMLHttpRequest()
    } else {
        _xhr = new ActiveXObject("Microsoft.XMLHTTP")
    }

    // 3.获得数据
    _xhr.onreadystatechange = function() {
       if (_xhr.readyState == 4 && _xhr.status == 200) {
            callback(eval("(" + _xhr.responseText + ")"))
        }
    }
    // 2.用_xhr请求数据
    _xhr.open("get",url)
    _xhr.send()

}

//判断一个元素是否含有某个className
function hasClass(ele,className) {
  return ele.className && new RegExp("(^|\\s)" + className + "(\\s|$)").test(ele.className);
}

// 如果想这样使用：el.hasClass('show'),可以用下列写法
Element.prototype.hasClass = function(className) {
    return this.className && new RegExp("(^|\\s)" + className + "(\\s|$)").test(this.className);
}

// 移除某个元素的某个类名
function removeClass(ele, className) {
	var reg = new RegExp("(^|\\s)" + className + "(\\s|$)");
	ele.className = ele.className.replace(reg, '');
}

// 阻止事件冒泡
function stopDefault(e) {
	if (e && e.preventDefault) {
		e.preventDefault();
	} else {
		window.event.returnValue = false;
	}
}

function tag(name ,elem) {
	return (elem||document).getElementsByTagName(name);
}

//在某一个元素前面插入元素
function before(parent,before,elem) {
	if(elem==null){
		elem=before;
		before=parent;
		parent=before.parentNode;
	}

	parent.insertBefore(elem,before);

}
      //在某一个元素后面插入元素
function append(parent,elem){

	    var elems = document.createElement(elem);
		elems.id="het";
		elems.innerHTML="nihao"
		parent.appendChild(elems);

}
  //移除某元素
function remove(elem){
	if(elem) elem.parentNode.removeChild(elem);
}
      //清空元素
function empty(elem){
	while(elem.firstChild){
		remove(elem.firstChild);
	}
}
//获取元素文本的内容的通用函数
function text(element) {
	var t="";
	//查看这个元素是否有子元素，没有的话就是一个单独的元素
	element=element.childNodes||element;
	for(var j=0;j<element.length;j++) {
		//console.log(element[j].nodeValue);
		t += element[j].nodeType == 3 ?
		element[j].nodeValue:text(element[j].childNodes);
		//console.log(j+","+t);
	}
	//console.log(t);
	return t;
}

//检查是否有用一个指定的特性
function hasAttribute(elem,name) {
	return elem.getAttribute(name)!=null;
}

 //获取和设置元素特性的值
 function attr(elem,name,value) {
 	//判断这是不是一个合法字符串
 	if(!name||name.constructor!=String) return '';
name={'for':"htmlFor",'class':"className"}[name]||name;

if(typeof value!='undefined'){
	elem[name]=value;
	if(elem.setAttribute){
		elem.setAttribute(name,value);
	}
}
return elem[name]||elem.getAttribute(name)||'';
 }
// 通过元素名称获得元素
 function tag(name ,elem) {
	return (elem||document).getElementsByTagName(name);
}
//查找相关元素的前一个兄弟元素的函数
function prev(elem) {
	do{
		elem=elem.previousSibling;
	}while(elem && elem.nodeType!=1);
	return elem;
}
//查找相关元素的下一个兄弟元素
function next(elem) {
	do{
		elem=elem.nextSibling;
	}while(elem &&elem.nodeType!=1)
	return elem;
}
//查找第一个子元素的函数
function first(elem) {
	elem=elem.firstChild;
	return elem &&elem.nodeType!=1 ?
	next(elem):elem;
}
//查找最后一个子元素函数
function last(elem) {
	elem=elem.lastChild;
	return elem&& elem.nodeType!=1 ?
	prev(elem):elem;
}
//查找父元素
function parent(elem,num) {
	num=num||1;
	for(var i=0;i<num;i++)
	if(elem!=null) elem=elem.parentNode;
	return elem;
}function cleanWhitespace(element) {
	element=element||document.documentElement;
	var cur=element.firstChild;
	while(cur!=null) {
              var nextSibling = cur.nextSibling;
		if(cur.nodeType==3 && !/\S/.test(cur.nodeValue)) { //这是一个文本节点，并且只包含空格  nodeType nodeValue 节点属性
			element.removeChild(cur);
		}else if(cur.nodeType==1) { //是一个node节点
			cleanWhitespace(cur);
		}
	   cur = nextSibling;
	}
}
```


