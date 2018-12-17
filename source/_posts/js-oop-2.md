---
title: js面向对象与继承
date: 2016-06-23 16:42:41
tags: [javascript]
categories: [前端, javascript]
description:
---


直接来代码
```js
function Person(name, age) {
	this.name = name;
	this.age = age;
}
Person.prototype.hi = function() {
	console.log('Hi,my name is' + this.name + ',I`m ' + this.age + ' years old now.');
};
Person.prototype.LESS_NUM = 2;
Person.prototype.ARMS_NUM = 2;
Person.prototype.walk = function() {
	console.log(this.name + 'is walking...');
};

function Student(name, age, className) {
	Person.call(this, name, age);
	this.className = className;
}
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
Student.prototype.hi = function() {
	console.log('Hi,my name is' + this.name + ',I`m ' + this.age + ' years old now,and from ' + this.className + '.');
};
Student.prototype.learn = function(subject) {
	console.log(this.name + ' is learning ' + subject + 'at ' + this.className + '.');
};

// test
var bosn = new Student('Bosn', 27, 'class 2,Grade 2');
bosn.hi();
console.log(bosn.LESS_NUM);
bosn.walk();
bosn.learn('math');
```

结果如下：

补充：Object.create是在ES5以后才开始支持的，处理优化时可以使用以下代码if (!Object.create)
```js
{
	Object.create = function(proto) {
		function F() {}
		F.prototype = proto;
		return new F;
	}
}
```



