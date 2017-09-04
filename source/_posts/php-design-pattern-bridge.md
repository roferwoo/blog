---
title: 桥接模式（Bridge Pattern）
date: 2017-04-18 18:48:55
categories: [PHP]
description: 桥接模式,Bridge Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**桥接模式(Bridge Pattern)：**将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种*对象结构型模式*，又称为*柄体(Handle and Body)模式*或*接口(Interface)模式*。

# 代码实例

``` php
// 抽象化角色
// 抽象化给出的定义，并保存一个对实现化对象的引用。
abstract class Abstraction
{
    /* 对实现化对象的引用 */
    protected $imp;
    // 某操作方法
    public function operation()
    {
        $this->imp->operationImp();
    }
}
// 修正抽象化角色
// 扩展抽象化角色，改变和修正父类对抽象化的定义。
class RefinedAbstraction extends Abstraction
{
    public function __construct(Implementor $imp)
    {
        $this->imp = $imp;
    }

    // 操作方法在修正抽象化角色中的实现
    public function operation()
    {
        echo 'RefinedAbstraction operation  ';
        $this->imp->operationImp();
    }
}

// 实现化角色
// 给出实现化角色的接口，但不给出具体的实现。
abstract class Implementor
{
    // 操作方法的实现化声明
    abstract public function operationImp();
}
// 具体化角色A
// 给出实现化角色接口的具体实现
class ConcreteImplementorA extends Implementor
{
    // 操作方法的实现化实现
    public function operationImp()
    {
        echo 'Concrete implementor A operation <br />';
    }
}
// 具体化角色B
// 给出实现化角色接口的具体实现
class ConcreteImplementorB extends Implementor
{
    // 操作方法的实现化实现
    public function operationImp()
    {
        echo 'Concrete implementor B operation <br />';
    }
}

// 使用
class Client
{
    public static function main()
    {
        $abstraction = new RefinedAbstraction(new ConcreteImplementorA());
        $abstraction->operation();

        $abstraction = new RefinedAbstraction(new ConcreteImplementorB());
        $abstraction->operation();
    }
}
Client::main();
```

# 模式分析

理解桥接模式，重点需要理解如何将抽象化(Abstraction)与实现化(Implementation)脱耦，使得二者可以独立地变化。

- **抽象化：**抽象化就是忽略一些信息，把不同的实体当作同样的实体对待。在面向对象中，将对象的共同性质抽取出来形成类的过程即为抽象化的过程。
- **实现化：**针对抽象化给出的具体实现，就是实现化，抽象化与实现化是一对互逆的概念，实现化产生的对象比抽象化更具体，是对抽象化事物的进一步具体化的产物。
- **脱耦：**脱耦就是将抽象化和实现化之间的耦合解脱开，或者说是将它们之间的强关联改换成弱关联，将两个角色之间的继承关系改为关联关系。桥接模式中的所谓脱耦，就是指在一个软件系统的抽象化和实现化之间使用关联关系（组合或者聚合关系）而不是继承关系，从而使两者可以相对独立地变化，这就是桥接模式的用意。

# 优点

桥接模式的优点:

分离抽象接口及其实现部分。
1. 桥接模式有时类似于多继承方案，但是多继承方案违背了类的单一职责原则（即一个类只有一个变化的原因），复用性比较差，而且多继承结构中类的个数非常庞大，桥接模式是比多继承方案更好的解决方法。
2. 桥接模式提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统。
3. 实现细节对客户透明，可以对用户隐藏实现细节。

# 缺点

桥接模式的缺点:

1. 桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程。
2. 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围具有一定的局限性。

# 适用环境

在以下情况下可以使用桥接模式：

1. 如果一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系，通过桥接模式可以使它们在抽象层建立一个关联关系。
2. 抽象化角色和实现化角色可以以继承的方式独立扩展而互不影响，在程序运行时可以动态将一个抽象化子类的对象和一个实现化子类的对象进行组合，即系统需要对抽象化角色和实现化角色进行动态耦合。
3. 一个类存在两个独立变化的维度，且这两个维度都需要进行扩展。
4. 虽然在系统中使用继承是没有问题的，但是由于抽象化角色和具体化角色需要独立变化，设计要求需要独立管理这两者。
5. 对于那些不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥接模式尤为适用。

# 总结

1. 桥接模式将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体(Handle and Body)模式或接口(Interface)模式。
2. 桥接模式包含如下四个角色：抽象类中定义了一个实现类接口类型的对象并可以维护该对象；扩充抽象类扩充由抽象类定义的接口，它实现了在抽象类中定义的抽象业务方法，在扩充抽象类中可以调用在实现类接口中定义的业务方法；实现类接口定义了实现类的接口，实现类接口仅提供基本操作，而抽象类定义的接口可能会做更多更复杂的操作；具体实现类实现了实现类接口并且具体实现它，在不同的具体实现类中提供基本操作的不同实现，在程序运行时，具体实现类对象将替换其父类对象，提供给客户端具体的业务操作方法。
3. 在桥接模式中，抽象化(Abstraction)与实现化(Implementation)脱耦，它们可以沿着各自的维度独立变化。
4. 桥接模式的主要优点是分离抽象接口及其实现部分，是比多继承方案更好的解决方法，桥接模式还提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统，实现细节对客户透明，可以对用户隐藏实现细节；其主要缺点是增加系统的理解与设计难度，且识别出系统中两个独立变化的维度并不是一件容易的事情。
5. 桥接模式适用情况包括：需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系；抽象化角色和实现化角色可以以继承的方式独立扩展而互不影响；一个类存在两个独立变化的维度，且这两个维度都需要进行扩展；设计要求需要独立管理抽象化角色和具体化角色；不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统。