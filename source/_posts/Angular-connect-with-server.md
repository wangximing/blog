title: Angular Conncet with Server
date: 2016-03-08 16:33:45
tags:
---

## Angular Conncet with Server

Angular与服务器通过`$http`通信。$http 服务简单的封装了浏览器原生的XMLHttpRequest对象。
在Angular中，可以通过使用 `$http`, `$resource`, `Restangular`与服务器交互。

### $http

$http返回一个promise对象

```js
var promise = $http({method: 'GET', url: '/api/users', params: {}})
```

`$http`还提供了一些快捷方法$http.get(),$http.delete()等。

### $resource
Angular 提供了一个 `$resource` 服务，用于创建资源对象，我们可以用$resource 方便的与 RESTful的服务端进行交互。


$resource的参数为$resourec('url', {paramDefaults}, {actions})
#### 定义一个resource
```js
//定义
var Users = $resource('/api/users/:id', {id: '@id'})

//简单使用
Users.get(); //get users list
Users.get({id: '333'}); // get /api/users/333
```

可以看出`$resource` 使用起来比`$http`使用起来方便好多。`$resource`还支持 save(), delete等方法。

* get类方法的参数如下 get({params, successHandle, errorHandle})
* 非get类方法的参数 save({params, postData, successHandle, errorHandle})

#### $resource的附加属性
 * $promise
 * $resolved

 > 这里不理解
 
#### 自定义$resource
前面的关于user的例子

* 可以用get方法获取一个user.
* 可以根据id获取users

那么就引申出两个问题

* 怎么使用PATCH方法来修改user的一个字段
* 怎么根据username来获取一个user

在这里就可以使用`$resource`的action 参数来自定义$resource
action的参数为`action1: {method:?, params:?, isArray:?, headers:?, ...}`

* 使用patch方法

	```js
	$resource('/api/users/:id', {id: '@id'}, {
		patch: {method: 'PATCH'}
	});
	```
* 根据username获取user

	```js
	$resource('/api/users/:id', {id: '@id'}, {
		getByName: {method: 'GET', url: '/api/users/username-:username, params: {username: '@username'}'}
	});
	```
	这个容易犯一个错误就是为了根据用户名获取重新定义一个resource，但其实访问的都是users资源，是同一个，所以不应该建两个resource.


### Restangular

是一个专门的Angular发RESTful的请求的库。
**需要新开一篇**
> * ETags 
> * If-NoneMatch

### 拦截器
如果我们要添加全局性的功能，如身份验证，错误处理的时候。就要用到拦截器。

拦截器的核心是服务工厂，通过向$httpProvider.intercetors数组中添加 factory service。

一共有四种拦截器，两种成功的，两种失败的

* request 请求成功
* response 响应失败
* requestError 请求失败 **这个有什么用？**
* responseError 响应失败

```js
angular.module('app').factory('AuthHandler', function ($q, $injector, loginUser) {
  var AuthHandler = {};
  AuthHandler.request = function (config) {
    if (config.data && config.data.$skipAuthHandler) {
      config.$skipAuthHandler = true;
      delete config.data.$skipAuthHandler;
    }
    if (config.params && config.params.$skipAuthHandler) {
      config.$skipAuthHandler = true;
      delete config.params.$skipAuthHandler;
    }

    config.headers.Authorization = loginUser.authorization();
    return $q.when(config);
  };
  AuthHandler.responseError = function (rejection) {
    if (rejection.status === 401 && rejection.config && !rejection.config.$skipAuthHandler) {
      $injector.get('$state').go('unauthorized');
    }
    return $q.reject(rejection);
  };
  return AuthHandler;
});

//使用
angular.module('app').config(function (RewriteHandlerProvider, $httpProvider) {
  $httpProvider.interceptors.push('AuthHandler');
});

```