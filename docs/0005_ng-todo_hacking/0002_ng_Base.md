# AngularJS Base

## 简单示例

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>
 
<div ng-app="">
     <p>名字 : <input type="text" ng-model="name"></p>
     <h1>Hello {{name}}</h1>
</div>
 
</body>
</html>
```

**标签属性说明：**

* `ng-app`: 指令定义一个 AngularJS 应用程序，这里可以认为是一个标签范围。
* `ng-model`: 指令把元素值（比如输入域的值）绑定到应用程序，可以认为是数据源。指令可以为应用数据提供状态值(invalid, dirty, touched, error):
  * `{{myForm.myAddress.$valid}}`
  * `{{myForm.myAddress.$dirty}}`
  * `{{myForm.myAddress.$touched}}`
* `ng-bind`: 指令把应用程序数据绑定到 HTML 视图，绑定数据源的数据。
* `ng-init`: 指令初始化 AngularJS 应用程序变量，键值对，分号分开键值对。
* `{{ expression }}`: AngularJS 表达式把数据绑定到 HTML，这与 ng-bind 指令有异曲同工之妙，可以包含文字、运算符和变量。
* `ng-repeat`: 指令会重复一个 HTML 元素。
* `ng-controller`: 指令定义了应用程序控制器，一个`ng-app`里面可以有多个控制器。

## JS绑定数据

```HTML
<div ng-app="myApp" ng-controller="myCtrl">
    <input ng-model="name">
    <h1>{{greeting}}</h1>
    <button ng-click='sayHello()'>点我</button>    
</div>
 
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.name = "Runoob";
    $scope.sayHello = function() {
        $scope.greeting = 'Hello ' + $scope.name + '!';
    };
});
</script>
```

* AngularJS 模块（Module） 定义了 AngularJS 应用。
* AngularJS 控制器（Controller） 用于控制 AngularJS 应用。
* ng-app指令指明了应用, ng-controller 指明了控制器。
* 当在控制器中添加 $scope 对象时，视图 (HTML) 可以获取了这些属性。

## ng-init数据定义及获取

* `ng-init="quantity=1;cost=5"`: `{{ quantity * cost }}`
* `ng-init="firstName='John';lastName='Doe'"`: `{{ firstName + " " + lastName }}`
* `ng-init="person={firstName:'John',lastName:'Doe'}"`: `{{ person.lastName }}`
* `ng-init="points=[1,15,19,2,40]"`: `{{ points[2] }}`

## 如何使用 Scope

* Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。
* Scope 是一个对象，有可用的方法和属性。
* Scope 可应用在视图和控制器上。
* 所有的应用都有一个 $rootScope，它可以作用在 ng-app 指令包含的所有 HTML 元素中。
* $rootScope 可作用于整个应用中。是各个 controller 中 scope 的桥梁。用 rootscope 定义的值，可以在各个 controller 中使用。

```
<div ng-app="myApp" ng-controller="myCtrl">

<h1>{{lastname}} 家族成员:</h1>

<ul>
    <li ng-repeat="x in names">{{x}} {{lastname}}</li>
</ul>

</div>

<script>
var app = angular.module('myApp', []);

app.controller('myCtrl', function($scope, $rootScope) {
    $scope.names = ["Emil", "Tobias", "Linus"];
    $rootScope.lastname = "Refsnes";
});
</script>
```

## 过滤器

其实就是管道了。

## Service服务

* 在 AngularJS 中，服务是一个函数或对象，可在你的 AngularJS 应用中使用。
* AngularJS 内建了30 多个服务：
  * `$location`
  * `$http`
  * `$timeout`
  * `$interval`
  * `...`

## Event事件

* `ng-click`: 指令定义了 AngularJS 点击事件。
* `ng-hide`: 指令用于设置应用部分是否可见。

```
<div ng-app="myApp" ng-controller="personCtrl">

<button ng-click="toggle()">隐藏/显示</button>

<p ng-hide="myVar">
名: <input type="text" ng-model="firstName"><br>
姓名: <input type="text" ng-model="lastName"><br>
<br>
Full Name: {{firstName + " " + lastName}}
</p>

</div>

<script>
var app = angular.module('myApp', []);
app.controller('personCtrl', function($scope) {
    $scope.firstName = "John",
    $scope.lastName = "Doe"
    $scope.myVar = false;
    $scope.toggle = function() {
        $scope.myVar = !$scope.myVar;
    };
});
</script>
```
 
## include

* 在 HTML 中，目前还不支持包含 HTML 文件的功能。
* 使用 AngularJS, 你可以使用 `ng-include` 指令来包含 HTML 内容。
