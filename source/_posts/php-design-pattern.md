---
title: PHP设计模式——简介
date: 2017-04-18 14:27:55
categories: [PHP]
---
# 简介
在我们的编程过程中，所遇到的大部分问题其实都已经被其他程序员碰到过，并一再的处理了；不论是使用什么编程语言。**设计模式**提取了共同问题，定义了经过测试的解决方案，并描述了可能的结果。

**设计模式**是一些可以*在项目中重复使用的解决方案*，是*解决一般性问题的通用方法*。

通俗来说，设计模式就是一些编程的套路。

面向对象的原则是**“组合优于继承”**，因为组合可以以多种方式动态的处理任务。虽然对象的组合会导致代码的可读性下降，但会让系统更加灵活，复用性更高。

**设计模式**是很多前辈花费大量精力总结的经验，是经过检验的高效的一系列对象组合方式；可以通过不同的语言来实现，下面特整理了一份PHP语言的设计模式实现，共12种经典的设计模式：

## 创建型模式

**创建型模式(Creational Pattern)**对类的实例化过程进行了抽象，能够将软件模块中对象的创建和对象的使用分离。为了使软件的结构更加清晰，外界对于这些对象只需要知道它们共同的接口，而不清楚其具体的实现细节，使整个系统的设计更加符合单一职责原则。

创建型模式*在创建什么(What)*，*由谁创建(Who)*，*何时创建(When)*等方面都为软件设计者提供了尽可能大的灵活性。创建型模式隐藏了类的实例的创建细节，通过隐藏对象如何被创建和组合在一起达到使整个系统独立的目的。

1. [简单工厂模式（Simple Factory Pattern）](php-design-pattern-simple-factory.html)
2. [工厂方法模式（Factory Method Pattern）](php-design-pattern-factory-method.html)
3. [抽象工厂模式（Abstract Factory Pattern）](php-design-pattern-abstract-factory.html)
4. [建造者模式（Builder Pattern）](php-design-pattern-builder.html)
5. [单例模式（Singleton Pattern）](php-design-pattern-singleton.html)

## 结构型模式

**结构型模式(Structural Pattern)**描述如何将类或者对 象结合在一起形成更大的结构，就像搭积木，可以通过 简单积木的组合形成复杂的、功能更为强大的结构。

结构型模式可以分为*类结构型模式*和*对象结构型模式*：

- 类结构型模式关心类的组合，由多个类可以组合成一个更大的系统，在类结构型模式中一般只存在继承关系和实现关系。
- 对象结构型模式关心类与对象的组合，通过关联关系使得在一 个类中定义另一个类的实例对象，然后通过该对象调用其方法。根据“合成复用原则”，在系统中尽量使用关联关系来替代继 承关系，因此大部分结构型模式都是对象结构型模式。

1. [适配器模式（Adapter Pattern）](php-design-pattern-adapter.html)
2. [桥接模式（Bridge Pattern）](php-design-pattern-bridge.html)
3. [组合模式（Composite Pattern）](php-design-pattern-composite.html)
4. [装饰模式（Decorator Pattern）](php-design-pattern-decorator.html)
5. [外观模式（Facade Pattern）](php-design-pattern-facade.html)
6. [享元模式（Flyweight Pattern）](php-design-pattern-flyweight.html)
7. [代理模式（Proxy Pattern）](php-design-pattern-proxy.html)

## 行为型模式

  1. [命令模式（Command Pattern）](php-design-pattern-command.html)
  2. [中介者模式（Mediator Pattern）](php-design-pattern-mediator.html)
  3. [观察者模式（Observer Pattern）](php-design-pattern-observer.html)
  4. [状态模式（State Pattern）](php-design-pattern-state.html)
  5. [策略模式（Strategy Pattern）](php-design-pattern-strategy.html)

设计模式的**宗旨就是：重用**；在面向对象中，*类是用于生成对象的代码模版*，而*设计模式是用于解决共性问题的代码模版*。遵循这样的模板，我们可以设快速地设计出优秀的代码。

> 注意，设计模式只是模板，不是具体的代码。它是为了代码复用，增加可维护性。

# 设计基础
在学习设计模式之前，有几个概念应该深入理解。
假设有一个这样的类：

``` php
class Person
{
    private $name = 'Human';

    public function getName()
    {
        return $this->name;
    }
}
```

## 变量保存对象

``` php
$man = new Person(); // 变量不仅能保存整形数字、数组和字符串等；还可以保存对象。
echo $man->getName();
```

## 传递对象参数

如果另一个类用到了`Person`类的属性或方法，可以直接传进去：
``` php
class Student
{
    public funciton __construct($person)
    {
        echo $person->getName();
    }
}
// 传递对象
$jack = new Student(new Person());
```

## 限定传递参数的类型

在传递参数的时候，就限定参数的类型：
``` php
class Student
{
    public function __construct(Person $person)
    {
        echo $person->getName();
    }
}
// 传递对象
$jack = new Student(new Person());
```
这样，传给`new Studen()`的参数必须是`Person`的实例。

## 用类属性保存对象

一种经常用到的做法，是用一个类的属性保存对象或对象集合。
``` php
class Life
{
    public $persons = [];

    public function addPerson(Person $person)
    {
        $this->persons[] = $person;
    }
}
$cls = new Life();
$cls->addPerson(new Person());
$cls->addPerson(new Person());
```

> 面向对象的设计模式都是围绕上面的情形，做一些组合之后形成的。

# 设计原则

设计模式有六大原则，这些原则是经过代码大神们不断总结的规律，目的是提高代码的复用性，降低耦合。

## 开闭原则

1988年，勃兰特·梅耶（Bertrand Meyer）在他的著作《面向对象软件构造（Object Oriented Software Construction）》中提出了开闭原则（Open Close Principle），它的原文是这样：“Software entities should be open for extension,but closed for modification”。

- **意思：**软件模块应该对扩展开放，对修改关闭。
- **举例：**在程序需要进行新增功能的时候，不能去修改原有的代码，而是新增代码，实现一个热插拔的效果（*热插拔：灵活的去除或添加功能，不影响到原有的功能*）。
- **目的：**为了使程序的扩展性好，易于维护和升级。

## 里氏代换原则

- **意思：**里氏代换原则（Liskov Substitution Principle）是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。
- **举例：**球类，原本是一种体育用品，它的衍生类有篮球、足球、排球、羽毛球等等，如果衍生类替换了基类的原本方法，如把体育用品改成了食用品（那么软件单位的功能受到影响），就不符合里氏代换原则。
- **目的：**对实现抽象化的具体步骤的规范。

## 依赖倒转原则

- **意思：**依赖倒转原则（Dependence Inversion Principle）即针对接口编程，而不是针对实现编程。
- **举例：**以计算机系统为例,无论主板、CPU、内存、硬件都是在针对接口设计的，如果针对实现来设计，内存就要对应到针对某个品牌的主板，那么会出现换内存需要把主板也换掉的尴尬。
- **目的：**降低模块间的耦合。

## 接口隔离原则

- **意思：**接口隔离原则（Interface Segregation Principle）即使用多个隔离的接口，比使用单个接口要好。
- **举例：**比如：登录，注册时属于用户模块的两个接口，比写成一个接口好。
- **目的：**提高程序设计灵活性。

## 迪米特法则

迪米特法则（Demeter Principle）也称最少知道原则，1987年秋天由美国Northeastern University的Ian Holland提出，被UML的创始者之一Booch等普及。后来，因为在经典著作《 The Pragmatic Programmer》而广为人知。

- **意思：**一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。
- **举例：**一个类公开的public属性或方法越多，修改时涉及的面也就越大，变更引起的风险扩散也就越大。
- **目的：**降低类之间的耦合，减少对其他类的依赖。

## 单一职责原则

单一职责原则（ Single responsibility principle ）由罗伯特·C·马丁（Robert C. Martin）于《敏捷软件开发：原则、模式和实践》一书中给出的。马丁表示此原则是基于汤姆·狄马克(Tom DeMarco)和Meilir Page-Jones的著作中的内聚性原则发展出的。

- **意思：**一个类只负责一个功能领域中的相应职责，或者可以定义为：就一个类而言，应该只有一个引起它变化的原因。
- **举例：**该原则意思简单到不需要举例！
- **目的：**类的复杂性降低，可读性提高，可维护性提高。