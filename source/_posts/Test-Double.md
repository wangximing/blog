title: Test Double
date: 2015-07-27 21:48:24
tags: test
---
## 什么是Test Double？为什么要用Test Double？
Test Double即是测试替身。
讲测试替身之前，先要明确两个概念。
`待测程序`和`测试依赖`。
待测程序就是要被测试的程序，测试依赖就是待测程序所依赖的程序。
为什么要使用测试替身：要测试“待测程序”，测试依赖也必须存在，这使得测试变得更复杂。

测试替身的概念： Test Double是一种让“待测程序”可以不依靠“测试依赖”而被单独测试的做法。

<!--more-->

## 简单理解：
Test Double有五种，分别是`Dummy Object`， `Test Stub`， `Test Spy`， `Fake Object`， `Mock Object`。
* Dummy：不包含实体的对象，在测试中需要传入但不会被用到的参数。
* Stub： 传回固定值。
* Spy： 类似Stub，但会记录盗用。
* Mock：提供Dummy， Stub，Spy的功能，开发人员看不到TestDouble的程序，只可以设定Mock一提供返回值，预期的调用等。
* Fake:通过提供接近原始对象单比较简单的实现。

## 区别：
从Test Double与本体之间的功能相似度来区分，
* Dummy Object根本没有实现，是[假到非常假]的替身。
* Stub有实现，但其实现方式通常是写死某个特想的返回值。
* Spy和Stub有类似的实现，但是Stub用来处理状态验证，
而Spy则是用来处理行为验证的。Spy会记录待测程序和测试依赖的行为互动
* Fake是与本尊行为非常接近的替身。差别在与Fake采用比较简单的方式类实现。
* Mock，Mock技术可以做到Dummy，Stub，Spy的功能，无法做到Fake，Mock可以自动产生Dummy，Stub，Spy这三种TEST Double。
参考资料：http://teddy-chen-tw.blogspot.hk/2014/09/test-double1.html