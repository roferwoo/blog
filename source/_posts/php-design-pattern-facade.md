---
title: 外观模式（Facade Pattern）
date: 2017-04-18 18:58:55
categories: [PHP]
description: 外观模式,Facade Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**外观模式(Facade Pattern)：**外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为*门面模式*，它是一种对象结构型模式。

# 代码实例

``` php
// 相机类
class Camera
{
    // 打开录像机
    public function turnOn()
    {
        echo 'Turning on the camera.<br />';
    }
    // 关闭录像机
    public function turnOff()
    {
        echo 'Turning off the camera.<br />';
    }
    /**
     * 转到录像机
     * @param <type> $degrees
     */
    public function rotate($degrees) {
        echo 'rotating the camera by ', $degrees, ' degrees.<br />';
    }
}
// 灯类
class Light
{
    // 开灯
    public function turnOn()
    {
        echo 'Turning on the light.<br />';
    }
    // 关灯
    public function turnOff()
    {
        echo 'Turning off the light.<br />';
    }
    // 换灯泡
    public function changeBulb()
    {
        echo 'changing the light-bulb.<br />';
    }
}
// 感应器
class Sensor
{
    // 启动感应器
    public function activate()
    {
        echo 'Activating the sensor.<br />';
    }
    // 关闭感应器
    public function deactivate()
    {
        echo 'Deactivating the sensor.<br />';
    }
    // 触发感应器
    public function trigger()
    {
        echo 'The sensor has been trigged.<br />';
    }
}

class Alarm
{
    // 启动警报器
    public function activate()
    {
        echo 'Activating the alarm.<br />';
    }
    // 关闭警报器
    public function deactivate()
    {
        echo 'Deactivating the alarm.<br />';
    }
    // 拉响警报器
    public function ring()
    {
        echo 'Ring the alarm.<br />';
    }
    // 停掉警报器
    public function stopRing()
    {
        echo 'Stop the alarm.<br />';
    }
}

// 门面类
class SecurityFacade
{
    /* 录像机 */
    private $_camera1, $_camera2;
    /* 灯 */
    private $_light1, $_light2, $_light3;
    /* 感应器 */
    private $_sensor;
    /* 警报器 */
    private $_alarm;

    public function __construct()
    {
        $this->_camera1 = new Camera();
        $this->_camera2 = new Camera();

        $this->_light1 = new Light();
        $this->_light2 = new Light();
        $this->_light3 = new Light();

        $this->_sensor = new Sensor();
        $this->_alarm = new Alarm();
    }

    public function activate()
    {
        $this->_camera1->turnOn();
        $this->_camera2->turnOn();

        $this->_light1->turnOn();
        $this->_light2->turnOn();
        $this->_light3->turnOn();

        $this->_sensor->activate();
        $this->_alarm->activate();
    }

    public  function deactivate()
    {
        $this->_camera1->turnOff();
        $this->_camera2->turnOff();

        $this->_light1->turnOff();
        $this->_light2->turnOff();
        $this->_light3->turnOff();

        $this->_sensor->deactivate();
        $this->_alarm->deactivate();
    }
}
// 使用
class Client {
    private static $_security;

    public static function main()
    {
        self::$_security = new SecurityFacade();
        self::$_security->activate();
    }
}
Client::main();
```

# 模式分析

根据“单一职责原则”，在软件中将一个系统划分为若干个子系统有利于降低整个系统的复杂性，一个常见的设计目标是使子系统间的通信和相互依赖关系达到最小，而达到该目标的途径之一就是引入一个外观对象，它为子系统的访问提供了一个简单而单一的入口。

- 外观模式也是“迪米特法则”的体现，通过引入一个新的外观类可以降低原有系统的复杂度，同时降低客户类与子系统类的耦合度。
- 外观模式要求一个子系统的外部与其内部的通信通过一个统一的外观对象进行，外观类将客户端与子系统的内部复杂性分隔开，使得客户端只需要与外观对象打交道，而不需要与子系统内部的很多对象打交道。
- 外观模式的目的在于降低系统的复杂程度。
- 外观模式从很大程度上提高了客户端使用的便捷性，使得客户端无须关心子系统的工作细节，通过外观角色即可调用相关功能。

# 优点

外观模式的优点：

1. 对客户屏蔽子系统组件，减少了客户处理的对象数目并使得子系统使用起来更加容易。通过引入外观模式，客户代码将变得很简单，与之关联的对象也很少。
2. 实现了子系统与客户之间的松耦合关系，这使得子系统的组件变化不会影响到调用它的客户类，只需要调整外观类即可。
3. 降低了大型软件系统中的编译依赖性，并简化了系统在不同平台之间的移植过程，因为编译一个子系统一般不需要编译所有其他的子系统。一个子系统的修改对其他子系统没有任何影响，而且子系统内部变化也不会影响到外观对象。
4. 只是提供了一个访问子系统的统一入口，并不影响用户直接使用子系统类。

# 缺点

外观模式的缺点：

1. 不能很好地限制客户使用子系统类，如果对客户访问子系统类做太多的限制则减少了可变性和灵活性。
2. 在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

# 适用环境

在以下情况下可以使用外观模式：

1. 要为一个复杂子系统提供一个简单接口时可以使用外观模式。该接口可以满足大多数用户的需求，而且用户也可以越过外观类直接访问子系统。
2. 客户程序与多个子系统之间存在很大的依赖性。引入外观类将子系统与客户以及其他子系统解耦，可以提高子系统的独立性和可移植性。
3. 在层次化结构中，可以使用外观模式定义系统中每一层的入口，层与层之间不直接产生联系，而通过外观类建立联系，降低层之间的耦合度。

# 总结

1. 在外观模式中，外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为门面模式，它是一种对象结构型模式。
2. 外观模式包含两个角色：外观角色是在客户端直接调用的角色，在外观角色中可以知道相关的(一个或者多个)子系统的功能和责任，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理；在软件系统中可以同时有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能。
3. 外观模式要求一个子系统的外部与其内部的通信通过一个统一的外观对象进行，外观类将客户端与子系统的内部复杂性分隔开，使得客户端只需要与外观对象打交道，而不需要与子系统内部的很多对象打交道。
4. 外观模式主要优点在于对客户屏蔽子系统组件，减少了客户处理的对象数目并使得子系统使用起来更加容易，它实现了子系统与客户之间的松耦合关系，并降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程；其缺点在于不能很好地限制客户使用子系统类，而且在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。
5. 外观模式适用情况包括：要为一个复杂子系统提供一个简单接口；客户程序与多个子系统之间存在很大的依赖性；在层次化结构中，需要定义系统中每一层的入口，使得层与层之间不直接产生联系。