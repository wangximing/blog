title: scss
date: 2015-11-17 21:00:59
tags:
---
css不是一种编程语言，sass为css加入了编程元素。
sass的后缀是.scss。sassy css。
### 编译风格
* nested 嵌套缩进的css代码，反映了HTML DOM 的结构。
* expended: 没有嵌套缩进的css
* compact ：
* compressed：

### 变量
以$开头在sass中被认为是变量
```
$font-color: #333;
```

### 导入
reset.scss
```sass
ol {
	margin:0
	padding:0
}
```
```sass
@import 'reset';
bodyy {
}
```
```sass
html, body, ul, ol {
  margin: 0;
  padding: 0;
}

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

### Mixin

```sass
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box { @include border-radius(10px); }
```
```
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```

### 继承

```
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}

```
```
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

### 运算 operators

```
article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```
