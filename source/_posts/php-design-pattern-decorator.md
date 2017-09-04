---
title: 装饰模式（Decorator Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 装饰模式,Decorator Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**装饰模式(Decorator Pattern)：**动态地给一个对象增加一些额外的职责(Responsibility)，就增加对象功能来说，装饰模式比生成子类实现更为灵活。其别名也可以称为*包装器(Wrapper)*，与适配器模式的别名相同，但它们适用于不同的场合。根据翻译的不同，装饰模式也有人称之为“油漆工模式”，它是一种对象结构型模式。

# 代码实例

``` php
// 抽象构件角色
interface Component
{
    public function operation();
}
// 装饰角色
abstract class Decorator implements Component
{
    protected  $_component;

    public function __construct(Component $component)
    {
        $this->_component = $component;
    }
    public function operation()
    {
        $this->_component->operation();
    }
}

// 具体装饰类A
class ConcreteDecoratorA extends Decorator
{
    public function __construct(Component $component)
    {
        parent::__construct($component);
 
    }
    public function operation()
    {
        parent::operation(); // 调用装饰类的操作
        $this->addedOperationA(); // 新增加的操作
    }
    // 新增加的操作A，即装饰上的功能
    public function addedOperationA()
    {
        echo 'Add Operation A <br />';
    }
}
// 具体装饰类B
class ConcreteDecoratorB extends Decorator
{
    public function __construct(Component $component)
    {
        parent::__construct($component);
    }
    public function operation()
    {
        parent::operation();
        $this->addedOperationB();
    }
    // 新增加的操作B，即装饰上的功能
    public function addedOperationB()
    {
        echo 'Add Operation B <br />';
    }
}

// 具体构件
class ConcreteComponent implements Component
{
 
    public function operation()
    {
        echo 'Concrete Component operation <br />';
    }
 
}
// 使用
class Client
{
    public static function main()
    {
        $component = new ConcreteComponent();
        $decoratorA = new ConcreteDecoratorA($component);
        $decoratorB = new ConcreteDecoratorB($decoratorA);
 
        $decoratorA->operation();
        $decoratorB->operation();
    }
}
Client::main();
```

# 模式分析

1. 与继承关系相比，关联关系的主要优势在于不会破坏类的封装性，而且继承是一种耦合度较大的静态关系，无法在程序运行时动态扩展。在软件开发阶段，关联关系虽然不会比继承关系减少编码量，但是到了软件维护阶段，由于关联关系使系统具有较好的松耦合性，因此使得系统更加容易维护。当然，关联关系的缺点是比继承关系要创建更多的对象。
2. 使用装饰模式来实现扩展比继承更加灵活，它以对客户透明的方式动态地给一个对象附加更多的责任。装饰模式可以在不需要创造更多子类的情况下，将对象的功能加以扩展。

# 优点

装饰模式的优点:

1. 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供比继承更多的灵活性。
2. 可以通过一种动态的方式来扩展一个对象的功能，通过配置文件可以在运行时选择不同的装饰器，从而实现不同的行为。
3. 通过使用不同的具体装饰类以及这些装饰类的排列组合，可以创造出很多不同行为的组合。可以使用多个具体装饰类来装饰同一对象，得到功能更为强大的对象。
4. 具体构件类与具体装饰类可以独立变化，用户可以根据需要增加新的具体构件类和具体装饰类，在使用时再对其进行组合，原有代码无须改变，符合“开闭原则”。

# 缺点

装饰模式的缺点:

1. 使用装饰模式进行系统设计时将产生很多小对象，这些对象的区别在于它们之间相互连接的方式有所不同，而不是它们的类或者属性值有所不同，同时还将产生很多具体装饰类。这些装饰类和小对象的产生将增加系统的复杂度，加大学习与理解的难度。
2. 这种比继承更加灵活机动的特性，也同时意味着装饰模式比继承更加易于出错，排错也很困难，对于多次装饰的对象，调试时寻找错误可能需要逐级排查，较为烦琐。

# 适用环境

在以下情况下可以使用装饰模式：

1. 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
2. 需要动态地给一个对象增加功能，这些功能也可以动态地被撤销。
3. 当不能采用继承的方式对系统进行扩充或者采用继承不利于系统扩展和维护时。不能采用继承的情况主要有两类：第一类是系统中存在大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长；第二类是因为类定义不能继承（如final类）.

# 总结

1. 装饰模式用于动态地给一个对象增加一些额外的职责，就增加对象功能来说，装饰模式比生成子类实现更为灵活。它是一种对象结构型模式。
2. 装饰模式包含四个角色：抽象构件定义了对象的接口，可以给这些对 象动态增加职责（方法）；具体构件定义了具体的构件对象，实现了 在抽象构件中声明的方法，装饰器可以给它增加额外的职责（方法）； 抽象装饰类是抽象构件类的子类，用于给具体构件增加职责，但是具 体职责在其子类中实现；具体装饰类是抽象装饰类的子类，负责向构 件添加新的职责。
3. 使用装饰模式来实现扩展比继承更加灵活，它以对客户透明的方式动 态地给一个对象附加更多的责任。装饰模式可以在不需要创造更多子 类的情况下，将对象的功能加以扩展。
4. 装饰模式的主要优点在于可以提供比继承更多的灵活性，可以通过一种动态的 方式来扩展一个对象的功能，并通过使用不同的具体装饰类以及这些装饰类的 排列组合，可以创造出很多不同行为的组合，而且具体构件类与具体装饰类可 以独立变化，用户可以根据需要增加新的具体构件类和具体装饰类；其主要缺 点在于使用装饰模式进行系统设计时将产生很多小对象，而且装饰模式比继承 更加易于出错，排错也很困难，对于多次装饰的对象，调试时寻找错误可能需 要逐级排查，较为烦琐。
5. 装饰模式适用情况包括：在不影响其他对象的情况下，以动态、透明的方式给 单个对象添加职责；需要动态地给一个对象增加功能，这些功能也可以动态地 被撤销；当不能采用继承的方式对系统进行扩充或者采用继承不利于系统扩展 和维护时。
6. 装饰模式可分为透明装饰模式和半透明装饰模式：在透明装饰模式中，要求客 户端完全针对抽象编程，装饰模式的透明性要求客户端程序不应该声明具体构 件类型和具体装饰类型，而应该全部声明为抽象构件类型；半透明装饰模式允 许用户在客户端声明具体装饰者类型的对象，调用在具体装饰者中新增的方法。