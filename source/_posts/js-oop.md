---
  title: js-oop
  date: 2016-06-25 10:37:11
  articleTitle: js面向对象
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---


今天看了一些js面向对象的视频，js做的事真的好多
1、// 模拟重载
2、// 调用子类的方法
3、// 链式调用
4、// 抽象类

代码奉上

```js
// 模拟重载
function Person() {
	var args = arguments;
	if (typeof args[0] === 'object' && args[0]) {
		if (args[0].name) {
			this.name = args[0].name;
		}
		if (args[0].age) {
			this.age = args[0].age;
		}
	} else {
		if (args[0]) {
			this.name = args[0];
		}
		if (args[1]) {
			this.age = args[1];
		}
	}
}

Person.prototype.toString = function() {
	var str = 'name=' + this.name + ',age=' + this.age;
	console.log(str)
	return str;
}

var bosn = new Person('Bosn', 27);
bosn.toString();
var bosnO = new Person({
	name: 'bosnO',
	age: 38
});
bosnO.toString();

// 调用子类的方法
function Person(name) {
	this.name = name;
}

function Student(name, className) {
	this.className = className;
	Person.call(this, name);
}
var bosn = new Student('Bosn', 'sw2');
Person.prototype.init = function() {

};
Student.prototype.init = function() {
	// do sth
	Person.prototype.init.apply(this, arguments);
}

// 链式调用
function classManager() {
	//
}
classManager.prototype.addClass = function(str) {
	console.log('Class:' + str + 'added');
	return this;
};
classManager.addClass('classA').addClass('classB').addClass('classC');

// 抽象类
function DetectorBase() {
	throw new Error('Abstract class can not be invoked directly!');
}
DetectorBase.detect = function() {
	console.log('Detection starting...');
};
DetectorBase.stop = function() {
	console.log('Detector stopping...');
};
DetectorBase.init = function() {
	throw new Error('Error');
};

function LinkDetector() {
	//
}
LinkDetector.prototype = Object.create(Detector.prototype);
LinkDetector.prototype.constructor = LinkDetector;
// add methods to LinkDetector...
```
