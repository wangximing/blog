title: AnjularJS
date: 2015-07-28 16:33:45
tags:
---
## AngularJS配置：
* 下载AngularJS源文件，`angular.js`, `angular-router.js`等必须使用的源文件。
* 在跟路径（可以是其他路径，建议根路径）新建index.html文件，index.html的目录，将会作为整个angular的根路径。
* 在index.html 中标记出ng-app="appname",和ng-views=" "。ng-app的 `appname`将会作为为整个angular的项目名称。在app.js和controller、service的配置文件中要保持一致。
* 新建app.js文件，app.js中写的是angular的配置信息。如router等。
* 新建一个Controller。
* 新建一个Service。
* 在index.html中引入angula.js angular-router.js 相关js。和自己实现的app.js ,controller 的js，service的js。

<!--more-->

## AngularJS启动机制：
一直有一个疑问就是：在index.html文件中引入了要用的js文件后，怎么就可以确定AngularJS已经配置好了，它是怎么管理前台的呢？怎么的就会启动AngularJS了呢？

查了一些资料后整理如下。

*	系统加载文档结构，加载所有引用的js。
* 文档加载完成后，angular.js自动执行去寻找 “ng-app”确认系统边界。
* 根据ng-app的名称去app.js里面寻找相同名称的模块，并注入。
* 根据模块名称寻找controller，service。
* 项目被AngularJS接手。
要注意的是index.js 和app.js 是默认名称，不可更改。

https://github.com/johnpapa/angular-styleguide/blob/master/i18n/zh-CN.md 很好的教程。

## AngularJS 小知识
* $location.path("要去页面的AngularJS路由") 可以实现controller内页面跳转。
* AngularJS scope,研究下AngularJS的scope。
> 有两个页面（页面A，页面B）绑定的是相同的controller（commonController），
在一个页面（页面A）中触发commonController.changePage()方法，
changePage()方法，先给scope赋值（$scope.user = user）,再跳转到页面B。
这时在页面B中无法获取$scope.user的值。
排除写错的可能后，页面也没有刷新。认为是scope范围的问题：
两个页面共用一个controller时，controller作用于不同页面的时候，$scope是不同的两个$scope。
* AngularJS多选（select）提供默认值。
> 注意ng-model的类型和ng-option中for和in中间的变量类型要一致。即`addedUser.employee`和`employee`都是employee。还有就是ng-model的值必须是ng-option的一个index的值，否则也会造成类型的不一致。
如下面的值。
```
-------------JavaScript------------
$scope.employees = data;
$scope.addedUser = {employee: data[0]};

----------HTML---------------------
<select ng-model="addedUser.employee" ng-options = "employee.name for employee in employees"></select>
```

```
----------HTML---------------------
<select ng-model="employee" ng-options = "employee.name for employee in employees"></select>
---------JavaScript----------------
$scope.employees = employees;// 从后台获取的api/employees
$scope.employee = employee;// 从后台获取的 api/employees/employee
//这样子获取的话AngularJS对数组和普通对象的值封装是不同的
console.log(employees[0]);//Object {class: "com.tw.core.model.Employee", gender: "男", id: 4, name: "周杰伦", role: "Coach"}
console.log(employee);//Object {id: 4, name: "周杰伦", gender: "男", role: "Coach", $$hashKey: "object:6"}
// 可以看到我们以为的一样其实是错的。
```
> [官网例子是这么写的](https://docs.angularjs.org/api/ng/directive/ngOptions)