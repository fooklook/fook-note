###AngularJSnote

本节描述AngularJS应用程序的三个组成部分，并解释它们如何映射到模型-视图-控制器设计模式：
####模板（Templates）

模板是您用HTML和CSS编写的文件，展现应用的视图。 您可给HTML添加新的元素、属性标记，作为AngularJS编译器的指令。 AngularJS编译器是完全可扩展的，这意味着通过AngularJS您可以在HTML中构建您自己的HTML标记！
####应用程序逻辑（Logic）和行为（Behavior）

应用程序逻辑和行为是您用JavaScript定义的控制器。AngularJS与标准AJAX应用程序不同，您不需要另外编写侦听器或DOM控制器，因为它们已经内置到AngularJS中了。这些功能使您的应用程序逻辑很容易编写、测试、维护和理解。
####模型数据（Data）

模型是从AngularJS作用域对象的属性引申的。模型中的数据可能是Javascript对象、数组或基本类型，这都不重要，重要的是，他们都属于AngularJS作用域对象。

AngularJS通过作用域来保持数据模型与视图界面UI的双向同步。一旦模型状态发生改变，AngularJS会立即刷新反映在视图界面中，反之亦然。

此外，AngularJS还提供了一些非常有用的服务特性：

1. 底层服务包括依赖注入，XHR、缓存、URL路由和浏览器抽象服务。
- 您还可以扩展和添加自己特定的应用服务。
- 这些服务可以让您非常方便的编写WEB应用。

AngularJS 模块（Module） 定义了 AngularJS 应用。

AngularJS 控制器（Controller） 用于控制 AngularJS 应用。

ng-app指令定义了应用, ng-controller 定义了控制器。

```html
<div ng-app="myApp" ng-controller="myCtrl">
名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}<br>
姓名：<span ng-bind='firstName + " " + lastName'></span>
</div>
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName= "John";
    $scope.lastName= "Doe";
});
</script> 
```

###AngularJS 通过 ng-directives 扩展了 HTML。
ng-app 指令定义一个 AngularJS 应用程序。

ng-model 指令把元素值（比如输入域的值）绑定到应用程序。

ng-bind 指令把应用程序数据绑定到 HTML 视图。

ng-init 指令初始化 AngularJS 应用程序变量。

```html
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>姓名： <span ng-bind="firstName + ' ' + lastName"></span></p>
</div> 
```
ng-repeat 指令对于集合中（数组中）的每个项会 克隆一次 HTML 元素。

```html
<div ng-app="" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">
<p>循环对象：</p>
<ul>
  <li ng-repeat="x in names">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
</div> 
```
ng-controller 指令定义了应用程序控制器。

```html
 <div ng-app="myApp" ng-controller="myCtrl">
名: <input type="text" ng-model="firstName"><br>
名: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}
</div>
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
</script> 
```
ng-include 指令来包含 HTML 内容

```html
<div class="container">
  <div ng-include="'model1.htm'"></div>
  <div ng-include="'model2.htm'"></div>
</div>
```
###AngularJS 表达式 与 JavaScript 表达式

类似于 JavaScript 表达式，AngularJS 表达式可以包含字母，操作符，变量。与 JavaScript 表达式不同，AngularJS 表达式可以写在 HTML 中。与 JavaScript 表达式不同，AngularJS 表达式不支持条件判断，循环及异常。与 JavaScript 表达式不同，AngularJS 表达式支持过滤器。
###Angular过滤器
过滤器可以使用一个管道字符（|）添加到表达式和指令中。

currency 	格式化数字为货币格式。

```html
<div ng-app="myApp" ng-controller="costCtrl">
<input type="number" ng-model="quantity">
<input type="number" ng-model="price">
<p>总价 = {{ (quantity * price) | currency }}</p>
</div> 
``` 
filter 	从数组项中选择一个子集。

```html
<div ng-app="myApp" ng-controller="namesCtrl">
<p><input type="text" ng-model="test"></p>
<ul>
  <li ng-repeat="x in names | filter:test | orderBy:'country'">
    {{ (x.name | uppercase) + ', ' + x.country }}
  </li>
</ul>
</div> 
```
orderBy 	根据某个表达式排列数组。

```html
<div ng-app="myApp" ng-controller="namesCtrl">
<ul>
  <li ng-repeat="x in names | orderBy:'country'">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
<div> 
```
lowercase 	格式化字符串为小写。    
uppercase 	格式化字符串为大写。

```html
 <div ng-app="myApp" ng-controller="personCtrl">
<p>姓名为 {{ lastName | lowercase }}</p>
</div> 
<div ng-app="myApp" ng-controller="personCtrl">
<p>姓名为 {{ lastName | uppercase }}</p>
</div> 
```
