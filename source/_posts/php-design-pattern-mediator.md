---
title: 中介者模式（Mediator Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 中介者模式,Mediator Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**中介者模式(Mediator Pattern)**定义：用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式又称为调停者模式，它是一种对象行为型模式。

# 代码实例

``` php
// 抽象中介类
abstract class Mediator
{
    abstract public function send($message, $colleague);
}
// 抽象同事类
abstract class Colleague
{
    private $_mediator = null;
    public function __construct($mediator)
    {
        $this->_mediator = $mediator;
    }
    public function send($message)
    {
        $this->_mediators->send($message, $this);
    }
    abstract public function notify($message);
}
// 具体中介类
class ConcreteMediator extends Mediator
{
    private $_colleague1 = null;
    private $_colleague2 = null;
    public function send($message, $colleague)
    {
        if ($colleague == $this->_colleague1) {
            $this->_colleague1->notify($message);
        } else {
            $this->_colleague2->notify($message);
        }
    }
    public function set($colleague1, $colleague2)
    {
        $this->_colleague1 = $colleague1;
        $this->_colleague2 = $colleague2;
    }
}
// 具体同事类
class Colleague1 extends Colleague
{
    public function notify($message)
    {
        echo "Colleague1 Message is :".$message."<br/>";
    }
}
// 具体同事类
class Colleague2 extends Colleague
{
    public function notify($message)
    {
        echo "Colleague2 Message is :".$message."<br/>";
    }
}

class Client
{
    public static funciton main()
    {
        $mediator = new ConcreteMediator();
        $c1 = new Colleague1($mediator);
        $c2 = new Colleague2($mediator);
        $mediator->set($c1, $c2);
        $c1->send('to c2 from c1');
        $c2->send('to c1 from c2');
    }
}
Client::main();
```

# 模式分析

中介者模式可以使对象之间的关系数量急剧减少。

中介者承担两方面的职责：

1. 中转作用（结构性）：通过中介者提供的中转作用，各个同事对象就不再需要显式引用其他同事，当需要和其他同事进行通信时，通过中介者即可。该中转作用属于中介者在结构上的支持。
2. 协调作用（行为性）：中介者可以更进一步的对同事之间的关系进行封装，同事可以一致地和中介者进行交互，而不需要指明中介者需要具体怎么做，中介者根据封装在自身内部的协调逻辑，对同事的请求进行进一步处理，将同事成员之间的关系行为进行分离和封装。该协调作用属于中介者在行为上的支持。

# 优点

中介者模式的优点：

1. 简化了对象之间的交互。
2. 将各同事解耦。
3. 减少子类生成。
4. 可以简化各同事类的设计和实现。

# 缺点

中介者模式的缺点：

1. 在具体中介者类中包含了同事之间的交互细节，可能会导致具体中介者类非常复杂，使得系统难以维护。

# 适用环境

在以下情况下可以使用中介者模式：

1. 系统中对象之间存在复杂的引用关系，产生的相互依赖关系结构混乱且难以理解。
2. 一个对象由于引用了其他很多对象并且直接和这些对象通信，导致难以复用该对象。
3. 想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。可以通过引入中介者类来实现，在中介者中定义对象。
4. 交互的公共行为，如果需要改变行为则可以增加新的中介者类。

# 总结

1. 中介者模式用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式又称为调停者模式，它是一种对象行为型模式。
2. 中介者模式包含四个角色：抽象中介者用于定义一个接口，该接口用于与各同事对象之间的通信；具体中介者是抽象中介者的子类，通过协调各个同事对象来实现协作行为，了解并维护它的各个同事对象的引用；抽象同事类定义各同事的公有方法；具体同事类是抽象同事类的子类，每一个同事对象都引用一个中介者对象；每一个同事对象在需要和其他同事对象通信时，先与中介者通信，通过中介者来间接完成与其他同事类的通信；在具体同事类中实现了在抽象同事类中定义的方法。
3. 通过引入中介者对象，可以将系统的网状结构变成以中介者为中心的星形结构，中介者承担了中转作用和协调作用。中介者类是中介者模式的核心，它对整个系统进行控制和协调，简化了对象之间的交互，还可以对对象间的交互进行进一步的控制。
4. 中介者模式的主要优点在于简化了对象之间的交互，将各同事解耦，还可以减少子类生成，对于复杂的对象之间的交互，通过引入中介者，可以简化各同事类的设计和实现；中介者模式主要缺点在于具体中介者类中包含了同事之间的交互细节，可能会导致具体中介者类非常复杂，使得系统难以维护。
5. 中介者模式适用情况包括：系统中对象之间存在复杂的引用关系，产生的相互依赖关系结构混乱且难以理解；一个对象由于引用了其他很多对象并且直接和这些对象通信，导致难以复用该对象；想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。