---
title: 组合模式（Composite Pattern）
date: 2017-04-18 18:38:55
categories: [PHP]
description: 组合模式,Composite Pattern,PHP,设计模式,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
# 模式定义

**组合模式（Composite）：**将对象组合成树形结构以表示“部分-整体”的层次结构。组合模式使用户对单个对象和组合对象的使用具有一致性。

# 代码实例

## 安全式的组合模式

``` php
// 抽象组件角色
interface Component
{
    // 返回自己的实例
    public function getComposite();
    // 示例方法
    public function operation();
}
// 树枝组件角色
class Composite implements Component
{
    private $_composites;

    public function __construct()
    {
        $this->_composites = array();
    }
    public function getComposite()
    {
        return $this;
    }
    // 示例方法，调用各个子对象的operation方法
    public function operation()
    {
        echo 'Composite operation begin:<br />';
        foreach ($this->_composites as $composite) {
            $composite->operation();
        }
        echo 'Composite operation end:<br /><br />';
    }
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component)
    {
        $this->_composites[] = $component;
    }
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component)
    {
        foreach ($this->_composites as $key => $row) {
            if ($component == $row) {
                unset($this->_composites[$key]);
                return TRUE;
            }
        }
 
        return FALSE;
    }
    // 聚集管理方法 返回所有的子对象
    public function getChild()
    {
       return $this->_composites;
    }
 
}

class Leaf implements Component
{
    private $_name;

    public function __construct($name)
    {
        $this->_name = $name;
    }
    public function operation()
    {
        echo 'Leaf operation ', $this->_name, '<br />';
    }
    public function getComposite()
    {
        return null;
    }
}
// 使用
class Client
{
    public static function main()
    {
        $leaf1 = new Leaf('first');
        $leaf2 = new Leaf('second');
 
        $composite = new Composite();
        $composite->add($leaf1);
        $composite->add($leaf2);
        $composite->operation();
 
        $composite->remove($leaf2);
        $composite->operation();
    }
}
Client::main();
```

## 透明式的组合模式

在*Composite*类里面声明所有的用来管理子类对象的方法。这样做的是好处是所有的组件类都有相同的接口。在客户端看来，树叶类和合成类对象的区别起码在接口层次上消失了，客户端可以同等的对待所有的对象。这就是**透明式的组合模式**。

``` php
// 抽象组件角色
interface Component
{
    // 返回自己的实例
    public function getComposite();
    // 示例方法
    public function operation();
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component);
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component);
    /**
     * 聚集管理方法 返回所有的子对象
     */
    public function getChild();
}
// 树枝组件角色
class Composite implements Component
{
    private $_composites;

    public function __construct()
    {
        $this->_composites = array();
    }
    public function getComposite()
    {
        return $this;
    }
    // 示例方法，调用各个子对象的operation方法
    public function operation()
    {
        echo 'Composite operation begin:<br />';
        foreach ($this->_composites as $composite) {
            $composite->operation();
        }
        echo 'Composite operation end:<br /><br />';
    }
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component)
    {
        $this->_composites[] = $component;
    }
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component)
    {
        foreach ($this->_composites as $key => $row) {
            if ($component == $row) {
                unset($this->_composites[$key]);
                return TRUE;
            }
        }

        return FALSE;
    }
    // 聚集管理方法 返回所有的子对象
    public function getChild()
    {
       return $this->_composites;
    }
 
}

class Leaf implements Component
{
    private $_name;

    public function __construct($name)
    {
        $this->_name = $name;
    }
    public function operation()
    {
        echo 'Leaf operation ', $this->_name, '<br />';
    }
    public function getComposite()
    {
        return null;
    }
    /**
     * 聚集管理方法 添加一个子对象,此处没有具体实现，仅返回一个FALSE
     * @param Component $component  子对象
     */
    public function add(Component $component)
    {
        return FALSE;
    }
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  此处没有具体实现，仅返回一个FALSE
     */
    public function remove(Component $component)
    {
        return FALSE;
    }
    /**
     * 聚集管理方法 返回所有的子对象 此处没有具体实现，返回null
     */
    public function getChild()
    {
       return null;
    }
}

// 使用
class Client
{
    public static function main()
    {
        $leaf1 = new Leaf('first');
        $leaf2 = new Leaf('second');

        $composite = new Composite();
        $composite->add($leaf1);
        $composite->add($leaf2);
        $composite->operation();

        $composite->remove($leaf2);
        $composite->operation();
    }
}
Client::main();
```

# 模式分析

# 优点

1. 简化客户代码；
2. 使得更容易增加新类型的组件。

# 缺点

使你的设计变得更加一般化，容易增加组件也会产生一些问题，那就是很难限制组合中的组件。

# 适用环境

1. 你想表示对象的部分-整体层次结构；
2. 你希望用户忽略组合对象和单个对象的不同，用户将统一地使用组合结构中的所有对象。

# 总结

