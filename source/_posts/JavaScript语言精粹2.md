title: JavaScript语言精粹（2）
date: 2016-06-22 11:33:45
tags: 前端
---
## 正则表达式

## 方法

### Array数组

* array.concat()  合并多个数组

	var c = a.concat(b);//不会改变a和b数组
	
* array.join(separator)

	把array构造成字符串，默认separator为「，」
* array.pop() 移除数组中的最后一个元素，并返回这个元素(被移除的元素)

* array.push() 添加数据到数组，并返回数组的长度

```
var a = [1, 2, 3],
var b = [4, 5];

var c = a.push(b, false);
//a [1, 2, 3, [4, 5], false]
//c 5
```

* array.reverse() 反转array的数组，并返回

```
 a = [1, 2];
 var b = a.reverse;
 //a,b 都是 [2, 1]
```

* array.shift() 移除并返回第一个元素
* array.slice(start, end) 从数组中截取一段end默认为数组长度，不会改变数组。
* array.splice(start, deleteCount, item..)。 从数组中移除元素，并用items替换。会改变元素。
* array.sort(compareFunction) 不能正确的给数字排序。
* array.unshift(item..)从头部插入并返回新数组的长度。

### Function 函数

* function.apply(this, arguments)

 apply方法调用function，传递 this对象（上下文）和参数列表。
 
### Number 数字

* number.toExponential(小数位数)。

 将number转换为指数形式，其中小数位数必须为0~20.

* number.toFixed(小数位数) 

	将数字转换为n为小数的字符串，小数位数默认为0， 可选范围为0~20。

* number.toPrecision(percision) 把数字转换为精度为n的字符串。
* number.toString()

### Object 对象

* object.hasOwnProperty(name)

	判断对象是否具有某属性
	
### RegExp 正则表达式

* regexp.exec(string)

 匹配成功将返回数组
 
 ```
 下标0  [0] 是匹配的字符串
 下标1  [1] 是第一个捕获文本
 下标2  [2] 是第二个捕获文本
 ```
* regexp.test(string) 匹配返回true，不匹配返回 false。

### String 字符串

* string.charAt(n) 返回第n个位置的字符
* string.charCodeAt(n) 返回第n个位置的字符码
* string.indexOf() 查找
* string.lastIndexOf() 查找
* string.match()
* string.replace(serarchValue, replaceValue).
* string.slice(start, end) 截取字符串
* string.subString(start, end)
* string.split(分隔符， limit)。

	将字符串分割成数组

## 糟粕

### typeOf
 * typeOf null  //object

### parseInt 

指定进制，不然如果数字的第一个是0，则会基于8进制来转换

### NaN

* typeOf NaN === 'number';

* 非数字转换成字符串时
```
+'0' //0
+'oops' //NaN

```

* NaN !== NaN //true