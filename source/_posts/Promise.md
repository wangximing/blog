title: Promise
date: 2016-01-11 00:01:38
tags: JavaScript Promise
---

## 简介
Promise是一个古老的**概念**（1976），又叫Deferred(延期的)，Future`java.util.concurrent.Future`。

## Promise in JavaScript
### WHY
 JavaScript是单线程，为了防止线程阻塞，在JS中有一种写法叫做**回调**

```javascript
   $.get('/users/123', function(user) {
       //这里可以对User进行一些操作
   })
```

这样就解决了JS单线程的问题了，但是又引出了一个新的问题 回调地狱

```javascript
  $.get('/users/123', function(class) {
       $.get('/class/4', function(school) {
           $.get('/schooles/123/chairman', function(class) {
               console.log('get data');
           })
       })
   })
```

回调地狱存在的明显的问题 难读 也就是不优雅而且也难测。然后为了解决这些问题，降低异步编程的复杂性，开发人员一直寻找简便的方法来处理异步操作，就出现了一个概念**Promise**和Promise的一系列的实现。Promise代表了一种可能会长时间运行而且不一定必须完整的操作的结果.

### WHAT
所谓Promise，字面上可以理解为"承诺"，就是说A调用B，B返回一个"承诺"给A，然后A就可以在写计划的时候这么写：当B返回结果给我的时候，A执行方案S1，反之如果B因为什么原因没有给到A想要的结果，那么A执行应急方案S2，这样一来，所有的潜在风险都在A的可控范围之内了。

Promise有三种状态
- `pending`: 初始状态， new
- `fulfilled`: 表示操作成功//success callback
- `rejected`: 表示操作失败//failure callback

### HOW
- `promise.then(success,failure)`,`promise.catch(failure)`

    `promise.catch(failure)`相当于`promise.then(null, failure)`

  ```javascript
    vm.promise1 = function() {
    $http.get('/users')
      .then(function(data) {
        console.log('success1');
        return $http.get('/users/success');
      }, function() {
        console.log('error1');
      })
      .then(function() {
        console.log('success2');
        return $http.get('/users/error');
      }, function() {
        console.log('error2');
      })
      .catch(function() {
        console.log('error3');
      })
      .finally(function() {
        console.log('finally');
      });
  }
  //success1
  //success2
  //GET http://localhost:3000/users/error 500 (Internal Server Error)
  //error3
  //finally
  ```

- 如果 Promise 被 reject，后面的 then 又没有指定失败回调，会找后面的失败回调。

  ```javascript
    $http.get('/users/error')
      .then(function(data) {
        console.log('success1');
      })
      .then(function() {
        console.log('success2');
      }, function() {
        console.log('error1');
      })
      .catch(function() {
        console.log('error2');
      })
      .finally(function() {
        console.log('finally');
      });

            //GET http://localhost:3000/users/error
        //view1.js:44 error2
        //view1.js:50 finally
  ```

- 如果Promise失败，那么失败回调后会调用下一个then的成功回调

  ```javascript
    $http.get('/users/error')
      .catch(function() {
        console.log('error1');
      })
      .then(function(data) {
        console.log('success1');
      })
      .finally(function() {
        console.log('finally');
      });
     //GET http://localhost:3000/users/error 500 (Internal Server Error)
     //error1
     // success1
     // finally
  ```

- 如果Promise失败且返回了一个Promise，在后面的then的参数里会被拆开。

  ```javascript
    $http.get('/users/error')
      .catch(function() {
        $http.get('/users/success')
      })
      .then(function(data) {
        console.log('success1');
      }, function() {
        console.log('error1')
      })
      .finally(function() {
        console.log('finally');
      });

    //GET http://localhost:3000/users/error 500 (Internal Server Error)
    //success1
    //finally
  ```

- angular的all, defer, reject方法
- 不仅仅是Ajax

## MORE
- [Promise的前世今生](http://alinode.aliyun.com/blog/5)
- [jQuery Deferred](http://api.jquery.com/category/deferred-object/)
- [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [jQuery的deferred对象详解 | 阮一峰](http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html)
- [Promises/A+](https://promisesaplus.com/#point-11)
- [回调地狱](http://callbackhell.com/)
