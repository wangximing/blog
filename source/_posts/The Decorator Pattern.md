title: 装饰者模式学习笔记
date: 2015-01-16 12:08:58
tags: 设计模式
---
from [Justin's Tech Blog](http://www.cnblogs.com/justinw/archive/2007/06/11/779356.html)

装饰者模式： 动态的给一个对象添加额外的职责，就增加功能来说，Decorator比增加子类更灵活
> 优先使用组合，而不是继承(Favor composition over inheritance);

####举一个饮品的记账系统来引出装饰者模式。
![decorator1](https://wangximing.github.io/pictures/decorator1.jpg)

Brverage是所有饮料的基类，cost()是抽象方法，
所有子类都需要定义自己的cost() 实现来返回特定饮料的价钱，
description变量也是在子类里赋值，表示特定饮料的描述信息。
getDescription()方法可以返回这个描述。

<!-- more -->
除了咖啡以外，炼乳，巧克力，砂糖，牛奶等调味品也是要单独收费的，所以调味品也是订单系统中重要的一部分。
于是。。。。。。

![decorator2](https://wangximing.github.io/pictures/decorator2.jpg)

这样的设计明显很不合适。于是又有了下面的设计方案。

![decorator3](https://wangximing.github.io/pictures/decorator3.jpg)

首先在基类里增加了表示是否包含特定调味品的布尔变量如milk, soy等，然后提供了一些has(get)和set方法来设置这些布尔值；其次在Beverage类里实现cost()方法来计算调味品的价钱。所有咖啡子类将仍然覆盖cost()方法，只是这次它们需要同时调用基类的cost()方法，以便获得咖啡加上调味品后的总价。  
看上去似乎这是一个不错的设计，那么下面我们再来给Beverage增加子类，如下图所示：

![decorator4](https://wangximing.github.io/pictures/decorator4.jpg)

基类的cost()方法将计算所有调味品的价钱(当然是只包括布尔值为true的调味品)，子类里的cost()方法将扩展其功能，以包含特定类型饮料的价钱。
OK! 现在我们似乎已经有了一个看上去还不错的设计，那么Central Perk的这个记账系统就按这个设计来实现就万事大吉了吗？等一下，还是让我们先从以前学习过的“找到系统中变化的部分，将变化的部分同其它稳定的部分隔开。”这个设计原则出发，重新推敲一下这个设计。
那么对于一家咖啡店来说，都有那些变化点呢？调味品的品种和价格会变吗？咖啡的品种和价格会变吗？咖啡和调味品的组合方式会变吗？YES! 对于一家咖啡店来说，这些方面肯定会经常发生改变的！那么，当这些改变发生的时候，我们的记账系统要如何应对呢？ 如果调味品发生改变，那么我们只能从代码的层次重新调整Beverage基类，这太糟糕了；如果咖啡发生改变，我们可以增加或删除一个子类即可，这个似乎还可以忍受；那么咖啡和调味品的组合方式发生改变呢？如果顾客点了一杯纯黑咖啡外加两份砂糖和一份巧克力，或者顾客点了一杯脱咖啡因咖啡(Decaf)外加三份炼乳和一份砂糖呢？我倒！突然意识到，上面的设计根本不支持组合一份以上同种调味品的情况，因为基类里的布尔值只能记录是否包含某种调味品，而并不能表示包含几份，连基本的功能需求都没有满足，看来这些开发者可以卷铺盖滚蛋了！(似乎他们改行去做炸弹更合适！)
好吧！让我们来接手这个设计！我们已经分析了前面设计的失败之处，我们应该实现支持调味品的品种和价格任意改变而不需要修改已有代码的设计；我们还要实现支持咖啡品种和价格任意改变而不需要修改已有代码的设计(这点上面的设计通过继承算是实现了)；还有就是支持咖啡和调味品的品种和份数任意组合而不需要修改已有代码的设计；还有就是代码重用越多越好了，内聚越高越好了，耦合越低越好了；(还有最重要的，报酬越高越好啦！)
看来我们要实现的目标还真不少，那么我们到底该怎么做呢？说实话，我现在也不知道！我们需要先去拜访一下今天的主角—装饰者模式，看看她能给我们带来什么惊喜吧！

#### 装饰着模式

我们还是先看一下官方的定义：
The Decorator Pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality. (装饰者模式可以动态地给一个对象增加其他职责。就扩展对象功能来说，装饰者模式比生成子类更为灵活。)
这里我们要重点注意那个dynamically（动态的），什么是动态？静态又是什么？这是我们要重点区分的地方，后面我们还会专门讨论这个问题。下面先看看装饰者模式的类图和顺序图：

![decorator5](https://wangximing.github.io/pictures/decorator5.gif)

Component（被装饰对象基类）  
l         定义对象的接口，可以给这些对象动态增加职责；
ConcreteComponent（具体被装饰对象）
l         定义具体的对象，Decorator可以给它增加额外的职责；
Decorator（装饰者抽象类）
l         维护一个指向Component实例的引用，并且定义了与Component一致的接口；
ConcreteDecorator（具体装饰者）
l         具体的装饰对象，给内部持有的具体被装饰对象增加具体的职责；
![decorator6](https://wangximing.github.io/pictures/decorator6.gif)


我们先来说说上面提到的动态和静态的问题，所谓动态是说可以在系统运行时(RunTime)动态给对象增加其它职责而不需要修改代码或重新编译；所谓静态是说必须通过调整代码(DesignTime)才能给对象增加职责,而且系统还需要重新编译；从具体技术层面来说，对象的组合和继承正好对应于前面的动态和静态，因为通过对象组合建立的交互关系不是在代码中(DesignTime)固定死的，而是在运行时(RunTime)动态组合的；而通过继承建立的关系是僵硬的难以改变的，因为它是在代码中(DesignTime)固定死了的，根本不存在运行时(RunTime)改变的可能。换个角度说：我们应该多使用对象组合来保持系统的运行时扩展性，尽量少使继承，因为继承让程序变得僵硬！这句话听着是不是很熟悉啊？恩！这就是我们前面文章里提过多次的一个设计原则：Favor composition over inheritance.（优先使用对象组合，而非类继承），更多的就不需要再解释了吧？
那么回到装饰者模式，跟前面介绍过的模式一样，装饰者同样是一个很简单的模式，特别是画出类图和顺序图之后，一切都很清楚明了。这里只有一个地方需要特殊强调一下：Decorator是装饰者模式里非常特殊的一个类，它既继承于Component【IS A关系】,又维护一个指向Component实例的引用【HAS A关系】，换个角度来说，Decorator跟Component之间，既有动态组合关系又有静态继承关系，WHY? 这里为什么要这么来设计？上面我们说过，组合的好处是可以在运行时给对象增加职责，Decorator【HAS A】Component的目的是让ConcreteDecorator可以在运行时动态给ConcreteComponent增加职责，这一点相对来说还比较好理解；那么Decorator继承于Component的目的是什么？在这里，继承的目的只有一个，那就是可以统一装饰者和被装饰者的接口，换个角度来说，不管是ConcretComponent还是ConcreteDecorator，它们都是 Component，用户代码可以把它们统一看作Component来处理，这样带来的更深一层的好处就是，装饰者对象对被装饰者对象的功能职责扩展对用户代码来说是完全透明的，因为用户代码引用的都是Component，所以就不会因为被装饰者对象在被装饰后，引用它的用户代码发生错误，实际上不会有任何影响，因为装饰前后，用户代码引用的都是Component类型的对象，这真是太完美了！装饰者模式通过继承实现统一了装饰者和被装饰者的接口，通过组合获得了在运行时动态扩展被装饰者对象的能力。
我们再举个生活中的例子，俗话说“人在衣着马在鞍”，把这就话用装饰者模式的语境翻译一下，“人通过漂亮的衣服装饰后，男人变帅了，女人变漂亮了；”。对应上面的类图，这里人对应于ConcreteComponent,而漂亮衣服则对应于ConcreteDecorator；换个角度来说，人和漂亮衣服组合在一起【HAS A】，有了帅哥或美女，但是他们还是人【IS A】，还要做人该做的事情，但是可能会对异性更有吸引力了(扩展功能)！
现在我们已经认识了装饰者模式，知道了动态关系和静态关系是怎么回事，是时候该解决咖啡店的问题了，从装饰者模式的角度来考虑问题，咖啡和调味品的关系应该是：咖啡是被装饰对象而调味品是装饰者，咖啡和调味品可以任意组合，但是不管怎么组合，咖啡还是咖啡！原来这么简单啊！具体看下面的类图

![decorator7](https://wangximing.github.io/pictures/decorator7.jpg)

如图所示，Beverage还是所有饮料的基类，它对应于装饰者模式类图里的Component,是所有被装饰对象的基类；HouseBlend, DarkRoast, Espresso, Decaf是具体的饮料(咖啡)种类，对应于前面的ConcreteComponent，即是具体的被装饰对象；CondimentDecorator对应于前面的Decorator，是装饰者的抽象类；而Milk，Mocha，Soy，Whip则都是具体的调味品，对于前面的ConcreteDecorator，也就是具体的装饰者。下面我们通过具体的代码再进一步理解一下基于装饰者模式的记账系统的实现.

饮品的基类（被装饰者）
```javascript
  //基类
  function Beverage(size) {
    this.size = size;
  }

  Beverage.prototype.getDiscription = function () {
    return 'Unknown Beverage';
  }

  Beverage.prototype.getSize = function (){
    return size;
  }

  Beverage.prototype.setSize = function (size){
    this.setSize = size;
  }
```


装饰者基类（继承饮料基类，并有一个饮料基类的属性）
```javascript
  function CondimentDecorator (beverage){
    this.beverage = beverage;
  }

  CondimentDecorator.prototype = Object.create(Beverage.prototype);
  CondimentDecorator.prototype.constructor = CondimentDecorator;
```


饮料类，继承Beverage

```javascript
function Decaf(beverage) {
  Beverage.call(this, beverage);
}

Decaf.prototype = Object.create(Beverage.prototype);
Decaf.prototype.constructor = Decaf;
//其他饮品类类类似
```

具体调味品类，继承CondimeDecorator类
```javascript
function Mocha(beverage) {
  CondimentDecorator.call(beverage);
}

Mocha.prototype = Object.create(Mocha.prototype);
Mocha.prototype.constructor = Mocha;

Mocha.prototype.getDescription = function () {
return beverage.GetDescription() + ',Mocha';
}

Mocha.prototype.cost = function () {
  return 10 + beverage.cost();
}
```

上面只是具体的实现代码，并没有具体的结果的演示。于是这里提供一个基于Jasmine的测试

```javscript
describe('Decaf', function() {
  it('#cost()', function () {
    var beverage = new Deaf();
    beverage = new Mocha(beverage);
    beverage = new Whip(beverage);
    expect(beverage.cost()).toBe(3.5);
    })
  });
```

#### 装饰模式的应用场景和优缺点

上面已经对装饰者模式做了比较详细的介绍，还是那句话，人无完人，模式也不是万能的，我们要用好设计模式来解决我们的实际问题，就必须熟知模式的应用场景和优缺点：
##### 装饰者模式的应用场景：
1.  想透明并且动态地给对象增加新的职责的时候。
2.  给对象增加的职责，在未来存在增加或减少可能。
3.  用继承扩展功能不太现实的情况下，应该考虑用组合的方式。
##### 装饰者模式的优点：
1.  通过组合而非继承的方式，实现了动态扩展对象的功能的能力。
2.  有效避免了使用继承的方式扩展对象功能而带来的灵活性差，子类无限制扩张的问题。
3.  充分利用了继承和组合的长处和短处，在灵活性和扩展性之间找到完美的平衡点。
4.  装饰者和被装饰者之间虽然都是同一类型，但是它们彼此是完全独立并可以各自独立任意改变的。
5  遵守大部分GRASP原则和常用设计原则，高内聚、低偶合。
##### 装饰者模式的缺点：
1.  装饰链不能过长，否则会影响效率。
2.  因为所有对象都是Component,所以如果Component内部结构发生改变，则不可避免地影响所有子类(装饰者和被装饰者)，也就是说，通过继承建立的关系总是脆弱地，如果基类改变，势必影响对象的内部，而通过组合(Decoator HAS A Component)建立的关系只会影响被装饰对象的外部特征。
3.只在必要的时候使用装饰者模式，否则会提高程序的复杂性，增加系统维护难度。


#### 相关设计原则

相信大家现在对装饰者模式都应该很清楚了吧！那么，就像我们在前面的文章里反复强调的一样，设计原则远比模式重要，学习设计模式的同时一定要注意体会设计原则的应用。这里我们再来看看装饰者模式里都符合那些主要的设计原则。
1.  Identify the aspects of your application that vary and separate them from what stays the same. (找到系统中变化的部分，将变化的部分同其它稳定的部分隔开。)
在装饰者模式的应用场景里变化的部分是Component的扩展功能。使用Decorator模式可以很好地将装饰者同被装饰者完全隔离开，我们可以任意改变ConcreteComponent或ConcreteDecorator，它们之间不会有任何相互影响。
2.  Program to an interface,not an implementation.（面向接口编程，而不要面向实现编程。）
Component和Decorator都是抽象类实现，其实相当于起到很好的接口隔离作用，在运行时具体操作的也是Component类型的变量引用，这完全是面向接口编程的。
3.  Favor composition over inheritance.（优先使用对象组合，而非类继承）
装饰者模式最成功的地方就是合理地使用了对象组合，通过组合灵活地扩展了Component的功能，所有的扩展的功能都是通过组合而非继承获得的，这从根本上决定了这种实现是高内聚低耦合的。
4.  Classes should be open for extension, but closed for modification (类应该对扩展开发，对修改关闭)
这是大名鼎鼎的OCP原则，我们在这个系列的第一篇【模式和原则】里就有专门的介绍。在装饰者模式里充分体现了OCP原则，在需要扩展Component的功能的时候，只需要实现一个新的特定的ConcreteDecorator即可，这完全是一种增量开发，不会对原来代码造成任何影响，对用户代码完全是透明的。
Code should be closed (to change) like the lotus flower in the evening, yet open (to extension) like the lotus flower in the morning. — From HFDP










本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利。
