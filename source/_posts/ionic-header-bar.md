title: ionic header bar
date: 2015-10-27 11:32:42
tags:
---
参考：[ionic 官方教程](http://learn.ionicframework.com/formulas/navigation-bar-vs-header-bar/);

我们可以用`ion-header-bar`,`ion-nav-bar`的任意一个来进行显示导航栏。
两者的区别在于，

`ion-header-bar`可以做进一步的样式设置扩展，比如说在两侧加上按钮等。

![导航栏扩展](/image/nav-extention.png)

`ion-nav-bar` 集成了`Ionic router`和历史路径的压栈。使用后可以回退到上一个浏览的页面。

![导航栏回退](/image/nav-back.png)
