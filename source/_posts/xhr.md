title: XHR 实践
date: 2016-03-10 16:33:45
tags:
---
* **跨域**：浏览器在全局层面禁止了页面在加载或执行与自身来源不同的域的任何脚本。

### 使用JSONP

jsonp的原理是通过<script>标签发起一个GET请求来取代XHR请求。JSONP生成一个<script>标签并插入到DOM中，然后浏览器会接管并向src属性所指向的地址发送请求。

### 使用CORS
* CORS （跨域资源共享，Corss Origin Resource Sharing）

### 使用代理服务器