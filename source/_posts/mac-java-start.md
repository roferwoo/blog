---
title: Mac下Java开发环境搭建
date: 2017-06-16 09:16:03
categories: [Mac, Java]
description: Mac,Java,JDK8,Java开发环境
---
最近公司要求几个Java项目，所以要把大学时期学习的Java捡起来，虽然在学校学习的是Java、JSP的一套，但毕业后就没写过了，一直做的PHP。也好借此机会重新学习一下吧，首先需要的当然是搭建开发环境，没有环境怎么玩！一步一步来吧~

## 一、安装JDK

**JDK**是Java语言的软件开发工具包，主要用于移动设备、嵌入式设备上的java应用程序。JDK是整个Java开发的核心，它包含了Java的运行环境，Java工具和Java基础的类库。
没有JDK的话，无法编译Java程序。

```
-> java -version
No Java runtime present, requesting install.
```

[官网下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)相应的JDK版本，然后安装（傻瓜式）。安装完成后，再打开Terminal，执行命令`java -version`即可查看到我们安装的JDK版本信息。

## 二、配置环境变量

```
-> /usr/libexec/java_home -v # 查看JDK真实路径
/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home
```

复制以上路径，编辑配置文件：`sudo vim /etc/profile`添加Java环境变量配置，如下：

```
# Java环境变量配置
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home"
CLASS_PATH=".:$JAVA_HOME/lib"
# 把JAVA添加到环境变量PATH中
export PATH="$PATH:$JAVA_HOME/bin"
```

## 三、配置服务器tomcat
[官网下载](http://tomcat.apache.org/download-90.cgi)对应版本，解压到自己想放置的位置。`sudo vim /etc/profile`添加Tomcat环境变量配置，如下：

```
# Tomcat环境变量配置
TOMCAT_HOME="/usr/local/tomcat"
export PATH="$PATH:$TOMCAT_HOME/bin"
```

设置完成后，执行`sudo chmod +x $TOMCAT_HOME/bin/*.sh`，之后可以使用`$TOMCAT_HOME/bin/startup.sh`启动Tomcat，默认端口为8080。浏览器访问*localhost:8080*即可看到Tomcat的相关信息界面。

## 四、开发工具IDE

- [IntelliJ IDEA](https://www.jetbrains.com/idea/)，[IntelliJ IDEA使用教程](http://www.phperz.com/special/83.html)。
- [Eclipse](http://www.eclipse.org/downloads/)
- [Eclipse Che](https://www.eclipse.org/che/getting-started/download/)
- [NetBeans](https://netbeans.org/downloads/index.html)