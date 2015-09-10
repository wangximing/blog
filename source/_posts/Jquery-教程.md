title: jQuery 教程
date: 2015-07-25 18:41:11
tags: JavaScript jQuery
---
## 简单介绍
### 什么是jQuery
jQuery是一个JavaScript库，它通过封装原生JavaScript函数得到了一整套定义好的方法。这些方法能有效地帮助Web设计师和开发者快捷编写和扩展JavaScript交互组件。jQuery没有提供任何新的功能，它最大的贡献是把JavaScript难懂难用的API整理成了易懂易用的jQuery语法，从而赢得了无数的用户。
### 为什么要用jQuery
* 开源
* 无与伦比的文档
* 更少的代码
* 链式调用
* 兼容各种浏览器
* 与CSS相近的属性选择器
* 不突兀的JavaScript（例如：事件绑定全部在JavaScript代码中完成，不需要在HTML中出现JavaScript代码）。

### 主要功能
* 处理DOM
* Ajax
* 事件处理
* 易于扩展
* 特殊效果（淡入淡出， 滑动）
* 动画

<!--more-->


下面分别简单介绍下jQuery的处理DOM、Ajax、事件处理的主要功能。

## 处理DOM

### jQuery选择器
####  Id选择器
```jQuery
$("#id")
```
#### 类选择器
```JavaScript
$(".class")
```
#### 标签选择器
```
$("标签名")
```
### 遍历
#### 父元素
```
.parent()
.closest()
```
#### 子元素
```
.children()
```
#### 筛选
```
.find()获得当前匹配元素集合中每个元素的后代，由选择器进行筛选
```
### 操作
#### 删除
```
.remove()
```
#### 替换
```
.replaceWith()
```
#### 取值赋值
```
.val()
.text()
```
#### 控制样式
```
.addClass()
.removeClass()
.toggleClass('active');
```
## Ajax
### Ajax基础
#### 什么是Ajax
Ajax是指在不需要刷新(重新加载)页面的情况下，允许客户端应用程序传送数据给服务器并从服务器获取数据的一组模式和技术。
#### coreApi
```
$.ajax()
```
option
```
data
error
success
error
type
url
cache
async
```
####Ajax-Related methods
```
$.load()
$.get()
$.post()
$.getScript()
$.getJson()
```
### Ajax+Deferred对象
在这里将deferred对象与Ajax回调结合，来介绍deffered对象与Ajax的联合使用。
#### 什么是Deffered
Deferred翻译成中文是延迟的意思，是jQuery 中用来处理回调函数的解决方案。延迟到摸个点在执行。
决绝jQUery的`回调地狱`的问题。
#### Deffered对象的方法（methods）
```
$.Deferred()生成一个deferred对象
.done() 执行成功时的回调
.fail() 执行失败时的回调
.resolve() 手动改变deferred对象 运行状态为“已成功”，从而触发.done（）回调。
.reject() 手动改变deferred对象运行状态为“已失败”，从而触发.fail()回调。
.then()
.always()

```
## 事件处理

### 系统事件
将系统事件分为四类
#### 文档和窗口事件
```
.ready()当HTML文档加载完成时触发
.load()HTML所有组件加载完成时触发
.unload()当浏览器窗口关闭或用户点击一个连接/在地址栏输入网址关闭本页面打开新页面时
.resize()当用户改变浏览器窗口大小时
.scroll()当用户滚动窗口时触发
.error()当http请求遇到错误时
```
#### 鼠标事件
#### 键盘事件
#### 表单事件
### 自定义事件
#### 自定义事件的定义与触发
* 定义 `$("#id").on(‘自定义事件名称’)`
* 触发 `this.trigger("name")`
#### 事件代理
##### 什么是事件代理

把事件绑定到父级元素。然后等待事件通过DOM冒泡到该元素时再执行

##### 事件代理的优点

减少绑定事件的数量，
减少意外发生次数.
#### 事件代理的语法
on()绑定
off()解绑
one()仅触发一次
####事件命名空间
