title: css
date: 2015-12-01 22:54:24
tags:
---
### 设置文字的text-overflow:ellipsis
有几个前提：
* 容器定宽 （width=100px）
* 去除空白 （white-space:nowrap）
* 溢出隐藏：overflow:hidden
* 文字省略： text-overflow:ellipsis

### AngularUI Bootstrap popover的使用
现在在一个滚动栏内使用popover，因为滚动栏设置的是 overflow=auto,导致popover多出的部分会藏在滚动栏里面。如图

解决的办法是利用popover的popover-append-to-body属性设置为true`popover-append-to-body=true`
