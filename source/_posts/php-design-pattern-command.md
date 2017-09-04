---
title: 命令模式（Command Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 命令模式,Command Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**命令模式(Command Pattern)：**将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种*对象行为型模式*，其别名为**动作(Action)模式**或**事务(Transaction)模式**。

# 代码实例

``` php
// 命令角色
interface Command
{
    // 执行方法
    public function execute();
}
// 具体命令角色
class ConcreteCommand implements Command
{
    private $_receiver;
    public function __construct(Receiver $receiver)
    {
        $this->_receiver = $receiver;
    }
    // 执行方法
    public function execute()
    {
        $this->_receiver->action();
    }
}
// 接收者角色
class Receiver
{
    /* 接收者名称 */
    private $_name;
    public function __construct($name)
    {
        $this->_name = $name;
    }
    // 行动方法
    public function action()
    {
        echo $this->_name, ' do action.<br />';
    }
}
// 请求者角色
class Invoker
{
    private $_command;
    public function __construct(Command $command)
    {
        $this->_command = $command;
    }
    public function action()
    {
        $this->_command->execute();
    }
}
// 使用
class Client
{
    public static function main()
    {
        $receiver = new Receiver('woophp');
        $command = new ConcreteCommand($receiver);
        $invoker = new Invoker($command);
        $invoker->action();
    }
}
 
Client::main();
```

# 模式分析

命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。

- 每一个命令都是一个操作：请求的一方发出请求，要求执行一个操作；接收的一方收到请求，并执行操作。
- 命令模式允许请求的一方和接收的一方独立开来，使得请求的一方不必知道接收请求的一方的接口，更不必知道请求是怎么被接收，以及操作是否被执行、何时被执行，以及是怎么被执行的。
- 命令模式使请求本身成为一个对象，这个对象和其他对象一样可以被存储和传递。
- 命令模式的关键在于引入了抽象命令接口，且发送者针对抽象命令接口编程，只有实现了抽象命令接口的具体命令才能与接收者相关联。

# 优点

命令模式的优点：

1. 降低系统的耦合度。
2. 新的命令可以很容易地加入到系统中。
3. 可以比较容易地设计一个命令队列和宏命令（组合命令）。
4. 可以方便地实现对请求的Undo和Redo。

# 缺点

命令模式的缺点：

1. 使用命令模式可能会导致某些系统有过多的具体命令类。因为针对每一个命令都需要设计一个具体命令类，因此某些系统可能需要大量具体命令类，这将影响命令模式的使用。

# 适用环境

在以下情况下可以使用命令模式：

1. 系统需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互。
2. 系统需要在不同的时间指定请求、将请求排队和执行请求。
3. 系统需要支持命令的撤销(Undo)操作和恢复(Redo)操作。
4. 系统需要将一组操作组合在一起，即支持宏命令。

# 总结

1. 在命令模式中，将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作模式或事务模式。
2. 命令模式包含四个角色：抽象命令类中声明了用于执行请求的`execute()`等方法，通过这些方法可以调用请求接收者的相关操作；具体命令类是抽象命令类的子类，实现了在抽象命令类中声明的方法，它对应具体的接收者对象，将接收者对象的动作绑定其中；调用者即请求的发送者，又称为请求者，它通过命令对象来执行请求；接收者执行与请求相关的操作，它具体实现对请求的业务处理。
3. 令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。命令模式使请求本身成为一个对象，这个对象和其他对象一样可以被存储和传递。
4. 命令模式的主要优点在于降低系统的耦合度，增加新的命令很方便，而且可以比较容易地设计一个命令队列和宏命令，并方便地实现对请求的撤销和恢复；其主要缺点在于可能会导致某些系统有过多的具体命令类。
5. 命令模式适用情况包括：需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互；需要在不同的时间指定请求、将请求排队和执行请求；需要支持命令的撤销操作和恢复操作，需要将一组操作组合在一起，即支持宏命令。