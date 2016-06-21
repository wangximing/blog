# JavaScript语言精粹

## 语法

### 标识符

* 由字母开头
* 不能使用保留字
* JavaScript不允许在__对象字面量__中，或者用点运算符提取对象属相时，使用保留字作为属性名。

### 数字

只有一个数字类型，在内部被表示为64为浮点数，没有分离出整数类型。

* 1=== 1.0
* NaN是一个数值，它表示一个不能产生正常结果的运算结果。NaN不等于任何值，包括它自己。
* Infinity 无穷大

### 字符串

* JavaScript中所有字符都是16位的。
* 字符串是不可变的，一旦字符串被创建，就无法改变它。
* 两个包含着完全相同的字符切字符顺序也相同的字符串被认为是相同的字符串。  `'a' + 'b' = 'ab'`

### 语句（Statements）

* 包在花括号中的语句不会创建作用域。
* 当 var语句被用字函数内部时，它定义的是这个函数的私有变量。
* falsy 
	
	> false,null,undefined, '', 0, NaN
* truly
	> true, 'false'等所有。
	
* 简单的for循环
* for..in循环
	for..in会枚举一个对象的所有属性名，并且通常要通过object.hasOwnProperty(variable)来约定这个属性名是该对象的成员还是来自原型链。
	
### 表达式（Expressions）

* typeof的值有 'number', 'string', 'boolean', 'undefined', 'function', 'object'。

	> * typeof null //object
	> * type [] //object

### 字面量

对象字面量是一种可以方面的按指定规格创建对象的表示法

* {}， []

### 函数 Functions


## 对象

简单数据类型： 数字，字符串，布尔值，null，undefined

对象： 函数，数组，对象。

### 对象字面量
 
 * {}

### 检索

* a.b
* a[b]
* || 运算符可以用来填充默认值 `var name = a.b || 'Bob'`
* && 可以用来避免错误 

	> * flight.a   //undefined
	> * flight.a.b  //error
	> * flight.a && flight.a.b  //undefined 
	
### 引用

对象通过引用来传递，它们永远不会被复制

### 原型

* set不会更新原型
* get会检索原型

### 反射

原型链中的任何属性都会产生值
  
  * typeof flight.toString  //function
  * typeof flight.constructor  //function

### 枚举

for..in 语句可与来遍历一个对象的所有属性，不过要排除原型链中的属性

```
	var name;
	
	for(name in object){
		if(typeof object[name] !== function && object.hasOWnProperty(name)){
			console.log(object[name])
		}
	}
```

* for..in属性名出现的是不固定的

### 删除

```
var a = {b: 'b', c: 'c'};

delete a.b;
``` 

### 减少全局变量的污染

使用命名空间减少全局遍历的污染


## 函数

### 函数对象

每个函数再创建的时候回附加两个属性，函数的上下文和实现函数行为的代码。

### 函数字面量

函数字面量创建的函数对象包含一个连接到外部的上下文，被称为闭包。

### 调用

* 函数调用时会传两个附加参数 this,arguemnts。
* 当实际参数个数比形参多时，超出的参数会被忽略。
* 实际参数比形参少时，缺失的值会被置为undefined。
* 四种调用模式：方法调用模式，函数调用模式，构造器调用模式，apply调用模式。

### 方法调用模式

当一个函数被保存为一个对象的属性时，成为方法。而这时的调用成为方法调用。

```
var object = {
	value: 0;
	doSomething : function() {
		this.value = 9;
	}
}
object.doSometing();
```

这时方法内的this指的是方法所在的object。所以它可以访问和修改对象的属性。

### 函数调用模式

当一个函数并非一个对象的属性时，它就是被当做函数来调用的。这时函数内的this绑定的是全局对象。

解决办法是把这个函数包一层并绑定到一个对象上

```
var a = add(4, 5)
object.double  = function(){
	var that = this;
	var helper = function(){
		return add(that.a, that.b);
	};
	
	helper();
}
```

### 构造器调用模式

使用new的方式调用。

```
var Person = function(firstName) {
  this.firstName = firstName;
};

Person.prototype.walk = function(){
  console.log("I am walking!");
};

function Student(firstName, subject) {
  Person.call(this, firstName);
  this.subject = subject;
}

Student.prototype = Object.create(Person.prototype);

Student.prototype.constructor = Student;
```

### apply调用模式

函数可以拥有方法，apply方法让我们传递一个参数数组给函数

```
var array = [3, 4];
app.apply(null, array);
```
apply 方法接受两个参数，第一个用于绑定this,第二个用于传递参数。

### 返回

* 函数总会有返回值，如果没有指定则返回undefined；

### 异常

### 扩充类型的功能

### 递归

### 作用域

* JavaScript有函数作用域，定义在函数内部的参数和变量在外部是不可见的。

### 闭包（P38）

* 作用域的好处是内部的函数可以访问定义它们的外部函数的参数和变量（除this，arguments）。
* 内部函数比外部函数有更长的生命周期。

```
var quo = function(status) {
	return {
		get_status: function(){
			return status;
		}
	}
}

var myQuo = quo('hello');
console.log(myQuo.get_status());
```

* 即使quo已经返回了，但get_status方法仍然享有访问quo对象status属性的权利
* get_status返回的并不是status参数的一个副本，返回的就是status属性本身。
* 函数可以访问它被创建是所处的上下文，这被成为闭包。

### 回调

### 模块
通过使用函数和闭包来构造模块

```
var a = function() {
	var x = 9;
	
	return {
		setX: function(input) {
			x = input;
		},
		getX: function() {
			return x;
		}
	}
}
```

### 级联

### 柯里化

把函数和传递给它的参数结合，生成一个新的函数。

### 记忆？？？

函数可以将先前的操作结果记录在某个对象中，从而避免无所谓的重复计算。这种优化被称为「记忆」

p44

斐波那契的例子

```
var fibonacci = function() {
	return n < 2 ? n : fibonacci(n-1) + fibonicci(n-2);
}

for(var i = 0; i <= 10; i++) {
	console.log(fibonacci(i));
}

fibonacci共被调用了453次，我们调用11次，本身调用442次


var fibonacci  = function() {
	var memo = [0, 1];
	
	var fib = function(n) {
		var result = memo[n];
		
		if(typeof result !== 'number') {
			result = fib(n-1) + fib(n-2);
			memo[n] = reuslt;
		}
		result result;
	};
	
	retuern fib;
}

计算29次，我们调用11次，自己调用18次。
```