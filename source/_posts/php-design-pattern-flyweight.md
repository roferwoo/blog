---
title: 享元模式（Flyweight Pattern）
date: 2017-04-18 18:59:55
categories: [PHP]
---
# 模式定义

**享元模式(Flyweight Pattern)：**运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。由于享元模式要求能够共享的对象必须是细粒度对象，因此它又称为轻量级模式，它是一种对象结构型模式。

# 代码实例

``` php
// 抽象享元角色
abstract class Flyweight
{
    abstract public function operation($state);
}
// 具体享元角色
class ConcreteFlyweight extends Flyweight
{
    private $_intrinsicState = null;
    public function __construct($state)
    {
        $this->_intrinsicState = $state;
    }
    public function operation($state)
    {
        echo 'ConcreteFlyweight operation, Intrinsic State = ' . $this->_intrinsicState
        . ' Extrinsic State = ' . $state . '<br />';
    }
}
// 不共享的具体享元，客户端直接调用
class UnsharedConcreteFlyweight extends Flyweight
{
    private $_intrinsicState = null;
    public function __construct($state)
    {
        $this->_intrinsicState = $state;
    }
    public function operation($state)
    {
        echo 'UnsharedConcreteFlyweight operation, Intrinsic State = ' . $this->_intrinsicState
        . ' Extrinsic State = ' . $state . '<br />';
    }
}

// 享元工厂角色
class FlyweightFactory
{
    private $_flyweights;
    public function __construct()
    {
        $this->_flyweights = array();
    }
    public function getFlyweigth($state)
    {
        if (isset($this->_flyweights[$state])) {
            return $this->_flyweights[$state];
        } else {
            return $this->_flyweights[$state] = new ConcreteFlyweight($state);
        }
    }
}
// 使用
class Client
{
    public static function main()
    {
        $flyweightFactory = new FlyweightFactory();
        $flyweight = $flyweightFactory->getFlyweigth('state A');
        $flyweight->operation('other state A');

        $flyweight = $flyweightFactory->getFlyweigth('state B');
        $flyweight->operation('other state B');

        /* 不共享的对象，单独调用 */
        $uflyweight = new UnsharedConcreteFlyweight('state A');
        $uflyweight->operation('other state A');
    }
}
Client::main();
```

# 模式分析

享元模式是一个考虑系统性能的设计模式，通过使用享元模式可以节约内存空间，提高系统的性能。

享元模式的核心在于享元工厂类，享元工厂类的作用在于提供一个用于存储享元对象的享元池，用户需要对象时，首先从享元池中获取，如果享元池中不存在，则创建一个新的享元对象返回给用户，并在享元池中保存该新增对象。

享元模式以共享的方式高效地支持大量的细粒度对象，享元对象能做到共享的关键是区分内部状态(Internal State)和外部状态(External State)。

- 内部状态是存储在享元对象内部并且不会随环境改变而改变的状态，因此内部状态可以共享。
- 外部状态是随环境改变而改变的、不可以共享的状态。享元对象的外部状态必须由客户端保存，并在享元对象被创建之后，在需要使用的时候再传入到享元对象内部。一个外部状态与另一个外部状态之间是相互独立的。

# 优点

享元模式的优点：

1. 享元模式的优点在于它可以极大减少内存中对象的数量，使得相同对象或相似对象在内存中只保存一份。
2. 享元模式的外部状态相对独立，而且不会影响其内部状态，从而使得享元对象可以在不同的环境中被共享。

# 缺点

享元模式的缺点：

1. 享元模式使得系统更加复杂，需要分离出内部状态和外部状态，这使得程序的逻辑复杂化。
2. 为了使对象可以共享，享元模式需要将享元对象的状态外部化，而读取外部状态使得运行时间变长。

# 适用环境

在以下情况下可以使用享元模式：

1. 一个系统有大量相同或者相似的对象，由于这类对象的大量使用，造成内存的大量耗费。
2. 对象的大部分状态都可以外部化，可以将这些外部状态传入对象中。
3. 使用享元模式需要维护一个存储享元对象的享元池，而这需要耗费资源，因此，应当在多次重复使用享元对象时才值得使用享元模式。

# 总结

1. 享元模式运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用，它是一种对象结构型模式。
2. 享元模式包含四个角色：抽象享元类声明一个接口，通过它可以接受并作用于外部状态；具体享元类实现了抽象享元接口，其实例称为享元对象；非共享具体享元是不能被共享的抽象享元类的子类；享元工厂类用于创建并管理享元对象，它针对抽象享元类编程，将各种类型的具体享元对象存储在一个享元池中。
3. 享元模式以共享的方式高效地支持大量的细粒度对象，享元对象能做到共享的关键是区分内部状态和外部状态。其中内部状态是存储在享元对象内部并且不会随环境改变而改变的状态，因此内部状态可以共享；外部状态是随环境改变而改变的、不可以共享的状态。
4. 享元模式主要优点在于它可以极大减少内存中对象的数量，使得相同对象或相似对象在内存中只保存一份；其缺点是使得系统更加复杂，并且需要将享元对象的状态外部化，而读取外部状态使得运行时间变长。
5. 享元模式适用情况包括：一个系统有大量相同或者相似的对象，由于这类对象的大量使用，造成内存的大量耗费；对象的大部分状态都可以外部化，可以将这些外部状态传入对象中；多次重复使用享元对象。