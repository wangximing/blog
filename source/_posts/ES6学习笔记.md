## let,content

* let 块级作用域
* 不变量提升，所以要先声明
* 不允许重复声明
* 暂时性死区

	只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响

```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError tmp is not defined
  let tmp;
}
```
上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错

* 函数不能声明在块级作用域中
* const声明一个只读的常量。一旦声明，常量的值就不能改变。
* const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

## 变量的解构赋值

* 解构 ： ES6允许通过一定模式，从数组和对象中取值，并进行赋值

```
var a = 1;
var b = 2;
var c = 3;

---->ES6	
var [a, b, c] = [1, 2, 3];
```

* 解构赋值允许指定默认值

## 字符串的扩展

* includes(), startsWith(), endsWith()

```
var s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

## Symbol

* ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

## Class

```
function Point(x,y){
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

--->ES6
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

-->ES6继承

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

* 一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

* 静态方法

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}
```

* mixin

## Decorator

## 严格模式

ES6自动采用严格模式，不论有没有加上 'use strict'

* 变量必须声明后再使用
* 函数的参数不能有同名属性，否则报错
* 不能使用with语句
* 不能对只读属性赋值，否则报错
* 不能删除不可删除的属性，否则报错
* 不能使用前缀0表示八进制数，否则报错
* 不能删除变量delete prop，会报错，只能删除属性delete global[prop]?
* eval不会在它的外层作用域引入变量
* eval和arguments不能被重新赋值
* arguments不会自动反映函数参数的变化
* 禁止this指向全局对象
* 增加了保留字（比如protected、static和interface）

## Module

### export

规定对外的接口，必须与模块内的变量建立一一对应的关系

```
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

---->另一种写法
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};

----->输出一个函数
export function multiply(x, y) {
  return x * y;
};

// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};
```

### import

```
import {firstName, lastName, year} from './profile';

import { lastName as surname } from './profile';


```
* import命令具有提升效果，会提升到整个模块的头部，首先执行。
* 整体导入

```
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}


// main.js

import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

---->整体导入
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
```
