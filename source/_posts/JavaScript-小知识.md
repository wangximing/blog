title: JavaScript 小知识
date: 2015-07-29 11:24:01
tags:
---

立即执行函数（IIFE -Immediately Invoked Function Expression）
```js
(function() {
    'use strict';

    angular
        .module('app')
        .factory('storage', storage);

    function storage() { }
})();
```
注：IIFE阻止了测试代码访问私有成员（正则表达式、helper函数等），这对于自身测试是非常友好的。然而你可以把这些私有成员暴露到可访问成员中进行测试，例如把私有成员（正则表达式、helper函数等）放到factory或是constant中。
