---
  title: angularjs中的$interpolate服务
  date: 2016-09-17 10:13:00
  tags: [angularjs]
  categories: [前端, angularjs]
  description:
---

`$interpolate`服务返回一个函数，用来在特定的上下文中运算表达式。
示例：
```html
<div ng-controller="myController">
  <input ng-model="to" type="email" placeholder="email" />
  <textarea ng-model="emailBody"></textarea>
  <pre>{{previewText}}</pre>
</div>
```

```js
angular.module('myApp', [])
  .controller('myController',['$scope','$interpolate',
    function($scope,$interpolate) {
      $scope.$watch('emailBody',function(body) {
        if(body) {
          var template = $interpolate(body);
          $scope.previewText = template({to:$scope.to})
        }
      })
    }
  ])
```

使用：在输入框中输入你的email地址，在文本框中输入{{to}}，previewText中的值即为to的值



