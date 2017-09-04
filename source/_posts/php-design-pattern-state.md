---
title: 状态模式（State Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 状态模式,State Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**状态模式(State Pattern)：**允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象(Objects for States)，状态模式是一种对象行为型模式。

# 代码实例

``` php
// 抽象状态角色
interface State
{
    public function handle(Context $context);
}
// 具体状态角色A 单例类
class ConcreteStateA implements State
{
    /* 唯一的实例 */
    private static $_instance = null;
    private function __construct()
    {
    }
    /* 静态工厂方法，返还此类的唯一实例 */
    public static function getInstance()
    {
        if (is_null(self::$_instance)) {
            self::$_instance = new self();
        }
        return self::$_instance;
    }
    public function handle(Context $context)
    {
        echo 'Concrete Sate A handle method<br />';
        $context->setState(ConcreteStateB::getInstance());
    }
}
// 具体状态角色B 单例类
class ConcreteStateB implements State
{
    /* 唯一的实例 */
    private static $_instance = null;
    private function __construct()
    {
    }
    /* 静态工厂方法，返还此类的唯一实例 */
    public static function getInstance()
    {
        if (is_null(self::$_instance)) {
            self::$_instance = new self();
        }
        return self::$_instance;
    }
    public function handle(Context $context)
    {
        echo 'Concrete Sate B handle method<br />';
        $context->setState(ConcreteStateA::getInstance());
    }
 
}
// 环境角色
class Context
{
    private $_state;
    // 默认为StateA
    public function __construct()
    {
        $this->_state = ConcreteStateA::getInstance();
    }
    public function setState(State $state)
    {
        $this->_state = $state;
    }
    public function request()
    {
        $this->_state->handle($this);
    }
}
// 使用
class Client
{
    public static function main()
    {
        $context = new Context();
        $context->request();
        $context->request();
        $context->request();
        $context->request();
    }
 
}
Client::main();
```

# 模式分析

1. 状态模式描述了对象状态的变化以及对象如何在每一种状态下表现出不同的行为。
2. 状态模式的关键是引入了一个抽象类来专门表示对象的状态，这个类我们叫做抽象状态类，而对象的每一种具体状态类都继承了该类，并在不同具体状态类中实现了不同状态的行为，包括各种状态之间的转换。

在状态模式结构中需要理解环境类与抽象状态类的作用：

1. 环境类实际上就是拥有状态的对象，环境类有时候可以充当状态管理器(State Manager)的角色，可以在环境类中对状态进行切换操作。
2. 抽象状态类可以是抽象类，也可以是接口，不同状态类就是继承这个父类的不同子类，状态类的产生是由于环境类存在多个状态，同时还满足两个条件：这些状态经常需要切换，在不同的状态下对象的行为不同。因此可以将不同对象下的行为单独提取出来封装在具体的状态类中，使得环境类对象在其内部状态改变时可以改变它的行为，对象看起来似乎修改了它的类，而实际上是由于切换到不同的具体状态类实现的。由于环境类可以设置为任一具体状态类，因此它针对抽象状态类进行编程，在程序运行时可以将任一具体状态类的对象设置到环境类中，从而使得环境类可以改变内部状态，并且改变行为。

# 优点

状态模式的优点：

1. 封装了转换规则。
2. 枚举可能的状态，在枚举状态之前需要确定状态种类。
3. 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。
4. 允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块。
5. 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。

# 缺点

状态模式的缺点：

1. 状态模式的使用必然会增加系统类和对象的个数。
2. 状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。
3. 状态模式对“开闭原则”的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态；而且修改某个状态类的行为也需修改对应类的源代码。

# 适用环境

在以下情况下可以使用状态模式：

1. 对象的行为依赖于它的状态（属性）并且可以根据它的状态改变而改变它的相关行为。
2. 代码中包含大量与对象状态有关的条件语句，这些条件语句的出现，会导致代码的可维护性和灵活性变差，不能方便地增加和删除状态，使客户类与类库之间的耦合增强。在这些条件语句中包含了对象的行为，而且这些条件对应于对象的各种状态。

# 总结

1. 态模式允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象，状态模式是一种对象行为型模式。
2. 状态模式包含三个角色：环境类又称为上下文类，它是拥有状态的对象，在环境类中维护一个抽象状态类State的实例，这个实例定义当前状态，在具体实现时，它是一个State子类的对象，可以定义初始状态；抽象状态类用于定义一个接口以封装与环境类的一个特定状态相关的行为；具体状态类是抽象状态类的子类，每一个子类实现一个与环境类的一个状态相关的行为，每一个具体状态类对应环境的一个具体状态，不同的具体状态类其行为有所不同。
3. 状态模式描述了对象状态的变化以及对象如何在每一种状态下表现出不同的行为。
4. 状态模式的主要优点在于封装了转换规则，并枚举可能的状态，它将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为，还可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数；其缺点在于使用状态模式会增加系统类和对象的个数，且状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱，对于可以切换状态的状态模式不满足“开闭原则”的要求。
5. 状态模式适用情况包括：对象的行为依赖于它的状态（属性）并且可以根据它的状态改变而改变它的相关行为；代码中包含大量与对象状态有关的条件语句，这些条件语句的出现，会导致代码的可维护性和灵活性变差，不能方便地增加和删除状态，使客户类与类库之间的耦合增强。