---
title: 抽象工厂模式（Abstract Factory Pattern）
date: 2017-04-18 17:58:09
categories: [PHP]
description: 抽象工厂模式,Abstract Factory Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**抽象工厂模式(Abstract Factory Pattern)**：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为*Kit模式*，属于对象创建型模式。

> **产品等级结构：**产品等级结构即产品的继承结构，如一个抽象类是电视机，其子类有海尔电视机、海信电视机、TCL电视机，则抽象电视机与具体品牌的电视机之间构成了一个产品等级结构，抽象电视机是父类，而具体品牌的电视机是其子类。
> **产品族：**在抽象工厂模式中，产品族是指由同一个工厂生产的，位于不同产品等级结构中的一组产品，如海尔电器工厂生产的海尔电视机、海尔电冰箱，海尔电视机位于电视机产品等级结构中，海尔电冰箱位于电冰箱产品等级结构中。

# 代码实例

``` php
// 抽象工厂
interface AbstractFactory
{
    // 创建等级结构为A的产品的工厂方法
    public function createProductA();
    // 创建等级结构为B的产品的工厂方法
    public function createProductB();
}
// 具体工厂1
class ConcreteFactory1 implements AbstractFactory
{
    public function createProductA()
    {
        return new ProductA1();
    }

    public function createProductB()
    {
        return new ProductB1();
    }
}
// 具体工厂2
class ConcreteFactory2 implements AbstractFactory
{
    public function createProductA()
    {
        return new ProductA2();
    }

    public function createProductB()
    {
        return new ProductB2();
    }
}

// 抽象产品A
interface AbstractProductA
{
    // 取得产品名
    public function getName();
}
// 抽象产品B
interface AbstractProductB
{
    // 取得产品名
    public function getName();
}

// 具体产品Ａ1
class ProductA1 implements AbstractProductA
{
    private $_name;

    public function __construct()
    {
        $this->_name = 'product A1';
    }

    public function getName()
    {
        return $this->_name;
    }
}
// 具体产品Ａ2
class ProductA2 implements AbstractProductA
{
    private $_name;

    public function __construct()
    {
        $this->_name = 'product A2';
    }

    public function getName()
    {
        return $this->_name;
    }
}

// 具体产品B1
class ProductB1 implements AbstractProductB
{
    private $_name;

    public function __construct()
    {
        $this->_name = 'product B1';
    }

    public function getName()
    {
        return $this->_name;
    }
}
// 具体产品B2
class ProductB2 implements AbstractProductB {
    private $_name;

    public function __construct()
    {
        $this->_name = 'product B2';
    }

    public function getName()
    {
        return $this->_name;
    }
}

// 使用
class Client
{
    // Main program.
    public static function main()
    {
        self::run(new ConcreteFactory1());
        self::run(new ConcreteFactory2());
    }

    /**
     * 调用工厂实例生成产品，输出产品名
     * @param   $factory    AbstractFactory     工厂实例
     */
    public static function run(AbstractFactory $factory)
    {
        $productA = $factory->createProductA();
        $productB = $factory->createProductB();
        echo $productA->getName(), '<br />';
        echo $productB->getName(), '<br />';
    }
 
}

Client::main();
```

# 模式分析

1. 抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态。
2. 抽象工厂模式与工厂方法模式最大的区别在于，工厂方法模式针对的是一个产品等级结构，而抽象工厂模式则需要面对多个产品等级结构，一个工厂等级结构可以负责多个不同产品等级结构中的产品对象的创建 。当一个工厂等级结构可以创建出分属于不同产品等级结构的一个产品族中的所有对象时，抽象工厂模式比工厂方法模式更为简单、有效率。

# 优点

1. 抽象工厂模式隔离了具体类的生成，使得客户并不需要知道什么被创建。由于这种隔离，更换一个具体工厂就变得相对容易。所有的具体工厂都实现了抽象工厂中定义的那些公共接口，因此只需改变具体工厂的实例，就可以在某种程度上改变整个软件系统的行为。另外，应用抽象工厂模式可以实现高内聚低耦合的设计目的，因此抽象工厂模式得到了广泛的应用。
2. 当一个产品族中的多个对象被设计成一起工作时，它能够保证客户端始终只使用同一个产品族中的对象。这对一些需要根据当前环境来决定其行为的软件系统来说，是一种非常实用的设计模式。
3. 增加新的具体工厂和产品族很方便，无须修改已有系统，符合“开闭原则”。

# 缺点

1. 在添加新的产品对象时，难以扩展抽象工厂来生产新种类的产品，这是因为在抽象工厂角色中规定了所有可能被创建的产品集合，要支持新种类的产品就意味着要对该接口进行扩展，而这将涉及到对抽象工厂角色及其所有子类的修改，显然会带来较大的不便。
2. 开闭原则的倾斜性（增加新的工厂和产品族容易，增加新的产品等级结构麻烦）。

# 适用环境

在以下情况下可以使用抽象工厂模式：

1. 一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有类型的工厂模式都是重要的。
2. 系统中有多于一个的产品族，而每次只使用其中某一产品族。
3. 属于同一个产品族的产品将在一起使用，这一约束必须在系统的设计中体现出来。
4. 系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

# 总结

1. 抽象工厂模式提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为Kit模式，属于对象创建型模式。
2. 抽象工厂模式包含四个角色：抽象工厂用于声明生成抽象产品的方法；具体工厂实现了抽象工厂声明的生成抽象产品的方法，生成一组具体产品，这些产品构成了一个产品族，每一个产品都位于某个产品等级结构中；抽象产品为每种产品声明接口，在抽象产品中定义了产品的抽象业务方法；具体产品定义具体工厂生产的具体产品对象，实现抽象产品接口中定义的业务方法。
3. 抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态。抽象工厂模式与工厂方法模式最大的区别在于，工厂方法模式针对的是一个产品等级结构，而抽象工厂模式则需要面对多个产品等级结构。
4. 抽象工厂模式的主要优点是隔离了具体类的生成，使得客户并不需要知道什么被创建，而且每次可以通过具体工厂类创建一个产品族中的多个对象，增加或者替换产品族比较方便，增加新的具体工厂和产品族很方便；主要缺点在于增加新的产品等级结构很复杂，需要修改抽象工厂和所有的具体工厂类，对“开闭原则”的支持呈现倾斜性。
5. 抽象工厂模式适用情况包括：一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节；系统中有多于一个的产品族，而每次只使用其中某一产品族；属于同一个产品族的产品将在一起使用；系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。