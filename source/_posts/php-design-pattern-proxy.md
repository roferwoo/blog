---
title: 代理模式（Proxy Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
---
# 模式定义

**代理模式(Proxy Pattern)：**给某一个对象提供一个代理，并由代理对象控制对原对象的引用。代理模式的英文叫做*Proxy*或*Surrogate*，它是一种对象结构型模式。

# 代码实例

``` php
// 抽象主题角色（Subject），定义了RealSubject和Proxy公用接口
interface Subject
{
    public function say();
    public function run();
}
// 真正的主题角色（RealSubject），定义了Proxy所代表的真实实体
class RealSubject implements Subject
{
    private $_name;
    public function __construct($name)
    {
        $this->_name = $name;
    }
    public function say()
    {
        echo $this->_name . '在吃饭<br>';
    }
    public function run()
    {
        echo $this->_name . '在跑步<br>';
    }
}
// 代理对象（Proxy）
class Proxy implements Subject
{
    private $_realSubject = null;
    public function __construct(RealSubject $realSubject = null)
    {
        if (empty($realSubject)) {
            $this->_realSubject = new RealSubject();
        } else {
            $this->_realSubject = $realSubject;
        }
    }
    public function say()
    {
        $this->_realSubject->say();
    }
    public function run()
    {
        $this->_realSubject->run();
    }
}
// 使用
class Client
{
    public static function main()
    {
        $rs = new RealSubject('张三');
        $proxy = new Proxy($rs);
        $proxy->say();
        $proxy->run();
    }
}
Client::main();
```

# 模式分析



# 优点

代理模式的优点：

1. 代理模式能够协调调用者和被调用者，在一定程度上降低了系统的耦合度。
2. 远程代理使得客户端可以访问在远程机器上的对象，远程机器可能具有更好的计算性能与处理速度，可以快速响应并处理客户端请求。
3. 虚拟代理通过使用一个小对象来代表一个大对象，可以减少系统资源的消耗，对系统进行优化并提高运行速度。
4. 保护代理可以控制对真实对象的使用权限。

# 缺点

代理模式的缺点：

1. 由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢。
2. 实现代理模式需要额外的工作，有些代理模式的实现非常复杂。

# 适用环境

根据代理模式的使用目的，常见的代理模式有以下几种类型：

1. 远程(Remote)代理：为一个位于不同的地址空间的对象提供一个本地的代理对象，这个不同的地址空间可以是在同一台主机中，也可是在另一台主机中，远程代理又叫做*大使(Ambassador)*。
2. 虚拟(Virtual)代理：如果需要创建一个资源消耗较大的对象，先创建一个消耗相对较小的对象来表示，真实对象只在需要时才会被真正创建。
3. Copy-on-Write代理：它是虚拟代理的一种，把复制（克隆）操作延迟到只有在客户端真正需要时才执行。一般来说，对象的深克隆是一个开销较大的操作，Copy-on-Write代理可以让这个操作延迟，只有对象被用到的时候才被克隆。
4. 保护(Protect or Access)代理：控制对一个对象的访问，可以给不同的用户提供不同级别的使用权限。
5. 缓冲(Cache)代理：为某一个目标操作的结果提供临时的存储空间，以便多个客户端可以共享这些结果。
6. 防火墙(Firewall)代理：保护目标不让恶意用户接近。
7. 同步化(Synchronization)代理：使几个用户能够同时使用一个对象而没有冲突。
8. 智能引用(Smart Reference)代理：当一个对象被引用时，提供一些额外的操作，如将此对象被调用的次数记录下来等。

# 总结

1. 在代理模式中，要求给某一个对象提供一个代理，并由代理对象控制对原对象的引用。代理模式的英文叫做Proxy或Surrogate，它是一种对象结构型模式。
2. 代理模式包含三个角色：抽象主题角色声明了真实主题和代理主题的共同接口；代理主题角色内部包含对真实主题的引用，从而可以在任何时候操作真实主题对象；真实主题角色定义了代理角色所代表的真实对象，在真实主题角色中实现了真实的业务操作，客户端可以通过代理主题角色间接调用真实主题角色中定义的方法。
3. 代理模式的优点在于能够协调调用者和被调用者，在一定程度上降低了系统的耦合度；其缺点在于由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢，并且实现代理模式需要额外的工作，有些代理模式的实现非常复杂。远程代理为一个位于不同的地址空间的对象提供一个本地的代表对象，它使得客户端可以访问在远程机器上的对象，远程机器可能具有更好的计算性能与处理速度，可以快速响应并处理客户端请求。
4. 如果需要创建一个资源消耗较大的对象，先创建一个消耗相对较小的对象来表示，真实对象只在需要时才会被真正创建，这个小对象称为虚拟代理。虚拟代理通过使用一个小对象来代表一个大对象，可以减少系统资源的消耗，对系统进行优化并提高运行速度。
5. 保护代理可以控制对一个对象的访问，可以给不同的用户提供不同级别的使用权限。