---
title: 策略模式（Strategy Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
---
# 模式定义

**策略模式(Strategy Pattern)：**定义一系列算法，将每一个算法封装起来，并让它们可以相互替换。策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。

策略模式是一种对象行为型模式。

# 代码实例

``` php
// 抽象策略角色，以接口实现
interface Strategy
{
    public function algorithmInterface();
}
// 具体策略角色A
class ConcreteStrategyA implements Strategy
{
    public function algorithmInterface()
    {
        echo 'algorithmInterface A<br />';
    }
}
// 具体策略角色B
class ConcreteStrategyB implements Strategy
{
    public function algorithmInterface()
    {
        echo 'algorithmInterface B<br />';
    }
}
// 具体策略角色C
class ConcreteStrategyC implements Strategy
{
    public function algorithmInterface()
    {
        echo 'algorithmInterface C<br />';
    }
}
// 环境角色
class Context
{
    /* 引用的策略 */
    private $_strategy;
    public function __construct(Strategy $strategy)
    {
        $this->_strategy = $strategy;
    }
    public function contextInterface()
    {
        $this->_strategy->algorithmInterface();
    }
}
// 使用
class Client
{
    public static function main()
    {
        $strategyA = new ConcreteStrategyA();
        $context = new Context($strategyA);
        $context->contextInterface();

        $strategyB = new ConcreteStrategyB();
        $context = new Context($strategyB);
        $context->contextInterface();

        $strategyC = new ConcreteStrategyC();
        $context = new Context($strategyC);
        $context->contextInterface();
    }
}
Client::main();
```

# 模式分析

1. 策略模式是一个比较容易理解和使用的设计模式，策略模式是对算法的封装，它把算法的责任和算法本身分割开，委派给不同的对象管理。策略模式通常把一个系列的算法封装到一系列的策略类里面，作为一个抽象策略类的子类。用一句话来说，就是“准备一组算法，并将每一个算法封装起来，使得它们可以互换”。
2. 在策略模式中，应当由客户端自己决定在什么情况下使用什么具体策略角色。
3. 策略模式仅仅封装算法，提供新算法插入到已有系统中，以及老算法从系统中“退休”的方便，策略模式并不决定在何时使用何种算法，算法的选择由客户端来决定。这在一定程度上提高了系统的灵活性，但是客户端需要理解所有具体策略类之间的区别，以便选择合适的算法，这也是策略模式的缺点之一，在一定程度上增加了客户端的使用难度。

# 优点

策略模式的优点：

1. 策略模式提供了对“开闭原则”的完美支持，用户可以在不修改原有系统的基础上选择算法或行为，也可以灵活地增加新的算法或行为。
2. 策略模式提供了管理相关的算法族的办法。
3. 策略模式提供了可以替换继承关系的办法。
4. 使用策略模式可以避免使用多重条件转移语句。

# 缺点

策略模式的缺点：

1. 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。
2. 策略模式将造成产生很多策略类，可以通过使用享元模式在一定程度上减少对象的数量。

# 适用环境

在以下情况下可以使用策略模式：

- 如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。
- 一个系统需要动态地在几种算法中选择一种。
- 如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。
- 不希望客户端知道复杂的、与算法相关的数据结构，在具体策略类中封装算法和相关的数据结构，提高算法的保密性与安全性。

# 总结

1. 在策略模式中定义了一系列算法，将每一个算法封装起来，并让它们可以相互替换。策略模式让算法独立于使用它的客户而变化，也称为政策模式。策略模式是一种对象行为型模式。
2. 策略模式包含三个角色：环境类在解决某个问题时可以采用多种策略，在环境类中维护一个对抽象策略类的引用实例；抽象策略类为所支持的算法声明了抽象方法，是所有策略类的父类；具体策略类实现了在抽象策略类中定义的算法。
3. 策略模式是对算法的封装，它把算法的责任和算法本身分割开，委派给不同的对象管理。策略模式通常把一个系列的算法封装到一系列的策略类里面，作为一个抽象策略类的子类。
4. 策略模式主要优点在于对“开闭原则”的完美支持，在不修改原有系统的基础上可以更换算法或者增加新的算法，它很好地管理算法族，提高了代码的复用性，是一种替换继承，避免多重条件转移语句的实现方式；其缺点在于客户端必须知道所有的策略类，并理解其区别，同时在一定程度上增加了系统中类的个数，可能会存在很多策略类。
5. 策略模式适用情况包括：在一个系统里面有许多类，它们之间的区别仅在于它们的行为，使用策略模式可以动态地让一个对象在许多行为中选择一种行为；一个系统需要动态地在几种算法中选择一种；避免使用难以维护的多重条件选择语句；希望在具体策略类中封装算法和与相关的数据结构。