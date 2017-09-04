---
title: Java学习笔记01—简介
date: 2017-07-07 09:05:07
categories: [Java]
description: Java,Java语法,Java学习笔记,MacBook Pro,Mac,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---

## 一、Java简介
**Java**是由Sun Microsystems公司于1995年5月推出的*Java面向对象程序设计语言*和*Java平台*的总称。由James Gosling和同事们共同研发，并在1995年正式推出。
Java分为三个体系：
- JavaSE（J2SE）（Java2 Platform Standard Edition，Java平台标准版）

JavaSE为开发普通桌面和商务应用程序提供的方案。该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发，如：Java版的扫雷应用。

- JavaEE（J2EE）（Java 2 Platform Enterprise Edition，Java平台企业版）

JavaEE是为开发企业环境下的应用程序提供的一套解决方案。该技术体系中包括的技术如*servlet*，*JSP*等，**主要针对于Web应用程序开发**。

- JavaME（J2ME）（Java 2 Platform Micro Edition，java平台微型版）

JavaME为开发电子消费产品和嵌入式设备提供的解决方案。该技术体系主要应用于小型电子消费产品，如：手机中的应用程序等。

>2005年6月，JavaOne大会召开，SUN公司公开Java SE 6。此时，Java的各种版本已经更名以取消其中的数字“2”：J2EE更名为Java EE, J2SE更名为Java SE，J2ME更名为Java ME。

## 二、主要特性

1. 简单
2. 面向对象
3. 分布式
4. 健壮
5. 跨平台
6. 解释型
7. 高性能

## 三、环境搭建
**JRE**（Java Runtime Environment，Java运行环境），包括Java虚拟机（JVM）和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。

**JDK**（Java Development Kit，Java开发工具包）JDK是提供给Java开发人员使用的，其中包含了Java的开发工具，也包括了*JRE*，所以安装了*JDK*，就不用再单独安装*JRE*。

>详见：[Mac下Java开发环境搭建](http://blog.wuzhiwei.cn/2017/06/16/mac-java-start.html)

## 四、Hello World
编写第一个Java程序，HelloWorld.java文件代码如下：
```
// public修饰的类名必须与文件名一致
public class HelloWorld {
    // 主函数，程序执行入口
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}

> javac HelloWorld.java # 编译
> java HelloWorld # 解释执行，注意“classpath”配置及字节码文件查找顺序
```

## 五、注释
- 单行注释，`// 注释文字`
- 多行注释，`/* 注释文字 */`
- 文档注释，`/** 注释文字 */`
