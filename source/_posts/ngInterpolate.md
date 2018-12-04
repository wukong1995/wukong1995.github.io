---
date: 2016-09-17 18:34:31
title: angularjs中的$interpolate服务
tags: ['angularjs']
categories: ['前端']
---
$interpolate服务返回一个函数，用来在特定的上下文中运算表达式。
示例：
html代码：

```html
<div ng-controller="myController">  
  <input ng-model="to" type="email" placeholder="email" />  
  <textarea ng-model="emailBody"></textarea>  
  <pre>{{previewText}}</pre>  
</div>  
```
js代码：
```javascript
angular.module('myApp', [])  
  .controller('myController',['$scope','$interpolate',  
    function($scope,$interpolate) {  
      $scope.$watch('emailBody',function(body) {  
        if(body) {  
          var template = $interpolate(body);  
          $scope.previewText = template({to:$scope.to})  
        }  
      })  
    }  
  ])  
```
使用：在输入框中输入你的email地址，在文本框中输入{{to}}，previewText中的值即为to的值
