title: Angular Service 学习笔记
date: 2016-03-02 16:33:45
tags:
---

## 创建服务的5中方式
* factory()
* service()
* contant()
* value()
* provider()


**service在Angular中是单例，只会被创建一次。**
### factory
```js
 angular.module('app')
 	.factory('serviceName', function () {
 		//在创建服务实例时被调用，只会被调用一次
 		var productService = {
 			filed: 'hello';,
 			method: function () {
 			}
 		}
 		return productService;
 	});

 angular.controller('Controller', function(serviceName) {
	serviceName.method();
});

```
### service()
service()函数会在创建实例时通过new关键字来实例化服务对象

```js
angular.service('serviceName', function () {
	this.method =  function() {};
	this.filed = 'hello';
});
angular.controller('Controller', function(serviceName) {
	serviceName.method();
});

```

### provider()
#### factory()和provider() ****
所有的factory()都是由$provider服务创建的, 假定传入的函数就是$get(), factory()就是provider()的简略形式。
> * $provider 和provider()的关系
> * $provider 和服务的关系

```js
angular.module('app')
  .provider('myService', {
  		$get: function (){}
  })
  .factory('myService', function () {
  });
```
#### provider()定义的服务，可用于config()
myService是定义provider的名字，myServerProvider是服务的提供者。
如下使用（未验证）；

```js
angular.module('app')
  .provider('myService', {
  		var message = '';
  		setMessage: function (message) {
  			message = message;
  		}
  		$get: function (){
  			console.log(message);
  		}
  })
  .config(function(myServiceProvider) {
  		myServiceProvider.setMessage('say hello');
  })
```

### constant(),value()
constant()和value()都是注册一个变量为服务，可以注入到应用的其他部分。
不同的是**constant()可以注入到config()中，而value()不可以**

### decorator()
decorator()可以对AngularJS的核心服务或者我们自己的服务进行里纳西，中断或者替换，Angular总的测试就是借助的$provider.decorator()建立的。

```
decorator('name', decoratorFn)
name: 将要拦截的服务名称
decoratorFn: 处理拦截的函数

```

```js
angular.module('app').config(function ($provider) {
	$provider.decorator('myService', function () {
		//怎么做还没弄明白
	})
}) ;
```

> 什么时候用factory()什么时候用service();
> service()-->factory()-->provider()
