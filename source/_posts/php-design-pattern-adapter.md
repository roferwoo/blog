---
title: 适配器模式（Adapter Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 适配器模式,Adapter Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**适配器模式(Adapter Pattern)：**将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为*包装器(Wrapper)*。适配器模式既可以作为*类结构型模式*，也可以作为*对象结构型模式*。

# 代码实例

## 类适配器模式

``` php
// 目标角色
interface Target
{
    // 源类也有的方法1
    public function sampleMethod1();
    // 源类没有的方法2
    public function sampleMethod2();
}

// 源角色 适配者
class Adaptee
{
    // 源类含有的方法
    public function sampleMethod1()
    {
        echo 'Adaptee sampleMethod1 <br />';
    }
}
// 类适配器角色
class Adapter extends Adaptee implements Target
{
    // 源类中没有sampleMethod2方法，在此补充
    public function sampleMethod2()
    {
        echo 'Adapter sampleMethod2 <br />';
    }
}
// 使用
class Client
{
    public static function main()
    {
        $adapter = new Adapter();
        $adapter->sampleMethod1();
        $adapter->sampleMethod2();
    }
}
Client::main();
```

## 对象适配器模式

``` php
// 目标角色
interface Target
{
    // 源类也有的方法1
    public function sampleMethod1();
    // 源类没有的方法2
    public function sampleMethod2();
}
// 源角色 适配者
class Adaptee
{
    // 源类含有的方法
    public function sampleMethod1()
    {
        echo 'Adaptee sampleMethod1 <br />';
    }
}
// 类适配器角色
class Adapter implements Target
{
    private $_adaptee;

    public function __construct(Adaptee $adaptee)
    {
        $this->_adaptee = $adaptee;
    }
    // 委派调用Adaptee的sampleMethod1方法
    public function sampleMethod1()
    {
        $this->_adaptee->sampleMethod1();
    }
    // 源类中没有sampleMethod2方法，在此补充
    public function sampleMethod2() {
        echo 'Adapter sampleMethod2 <br />';
    }
 
}
// 使用
class Client
{
    public static function main()
    {
        $adaptee = new Adaptee();
        $adapter = new Adapter($adaptee);
        $adapter->sampleMethod1();
        $adapter->sampleMethod2();
    }
}
Client::main();
```

# 模式分析

# 优点

1. 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码。
2. 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性。
3. 灵活性和扩展性都非常好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类，完全符合“开闭原则”。

*类适配器模式*还具有如下优点：
由于适配器类是适配者类的子类，因此可以在适配器类中置换一些适配者的方法，使得适配器的灵活性更强。
*对象适配器模式*还具有如下优点：
一个对象适配器可以把多个不同的适配者适配到同一个目标，也就是说，同一个适配器可以把适配者类和它的子类都适配到目标接口。

# 缺点

*类适配器模式*的缺点如下：
对于Java、C#等不支持多重继承的语言，一次最多只能适配一个适配者类，而且目标抽象类只能为抽象类，不能为具体类，其使用有一定的局限性，不能将一个适配者类和它的子类都适配到目标接口。
*对象适配器模式*的缺点如下：
与类适配器模式相比，要想置换适配者类的方法就不容易。如果一定要置换掉适配者类的一个或多个方法，就只好先做一个适配者类的子类，将适配者类的方法置换掉，然后再把适配者类的子类当做真正的适配者进行适配，实现过程较为复杂。

# 适用环境

在以下情况下可以使用适配器模式：

1. 系统需要使用现有的类，而这些类的接口不符合系统的需要。
2. 想要建立一个可以重复使用的类，用于与一些彼此之间没有太大关联的一些类，包括一些可能在将来引进的类一起工作。

# 总结

1. 结构型模式描述如何将类或者对象结合在一起形成更大的结构。
2. 适配器模式用于将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。
3. 适配器模式包含四个角色：目标抽象类定义客户要用的特定领域的接口；适配器类可以调用另一个接口，作为一个转换器，对适配者和抽象目标类进行适配，它是适配器模式的核心；适配者类是被适配的角色，它定义了一个已经存在的接口，这个接口需要适配；在客户类中针对目标抽象类进行编程，调用在目标抽象类中定义的业务方法。
4. 在类适配器模式中，适配器类实现了目标抽象类接口并继承了适配者类，并在目标抽象类的实现方法中调用所继承的适配者类的方法；在对象适配器模式中，适配器类继承了目标抽象类并定义了一个适配者类的对象实例，在所继承的目标抽象类方法中调用适配者类的相应业务方法。
5. 适配器模式的主要优点是将目标类和适配者类解耦，增加了类的透明性和复用性，同时系统的灵活性和扩展性都非常好，更换适配器或者增加新的适配器都非常方便，符合“开闭原则”；类适配器模式的缺点是适配器类在很多编程语言中不能同时适配多个适配者类，对象适配器模式的缺点是很难置换适配者类的方法。
6. 适配器模式适用情况包括：系统需要使用现有的类，而这些类的接口不符合系统的需要；想要建立一个可以重复使用的类，用于与一些彼此之间没有太大关联的一些类一起工作。