---
title: Mac下配置iTerm2支持rz/sz命令
date: 2017-07-07 09:21:18
categories: [iTerm2]
description: iTerm2,rz,sz,快捷键,iTerms快捷建,MacBook Pro,Mac,Tools,Mac开发常用工具,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
刚要进行Hive的ETL操作，需要将本地的一些脚本、配置文件上传到服务器（经过跳板机）执行。了解到*iTerm2*可以通过`rz`、`sz`命令实现文件的上传下载，极其方便（`scp`命令参数多，不容记忆！）。记录一下Mac下的配置过程。

>`scp`命令格式。
>上传：`scp -r local_folder remote_username@remote_ip:remote_folder`或`scp -r local_folder remote_ip:remote_folder`
>下载：`scp -r remote_username@remote_ip:remote_folder local_folder`

## 一、安装lrzsz
安装*lrzsz*前必须先安装*iTerm2*，使用*brew*命令如下：
```
brew install lrzsz
```

## 二、下载zmoden
访问[https://github.com/mmastrac/iterm2-zmodem](https://github.com/mmastrac/iterm2-zmodem)，将*iterm2-send-zmoden.sh*和*iterm2-recv-zmoden.sh*脚本下载下来，并放到*/usr/local/bin*目录下，赋予脚本执行权限。或通过命令行操作：
```
cd /usr/local/bin
sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-send-zmodem.sh
sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-recv-zmodem.sh
sudo chmod 777 /usr/local/bin/iterm2-*
```

## 三、配置iTerm2
打开*iTerm2*，点击*preferences->profiles->左侧选择相应的profile->advanced->triggers->Edit*，添加如下triggers：

Regular Expression|Action|Parameters|Instant
:-------------|:------------|:-------------------|:-----
\\\*\\\*B0100|Run Silent Coprocess|/usr/local/bin/iterm2-send-zmodem.sh|checked
\\\*\\\*B00000000000000|Run Silent Coprocess|/usr/local/bin/iterm2-recv-zmodem.sh|checked

## 四、使用rz/sz

- 将文件传到远端服务器。
  1. 在远端服务器上输入`rz`，回车；
  2. 选择本地要上传的文件；
  3. 等待上传。

- 从远端服务器下载文件。
  1. 在远端服务器输入`sz filename filename1 ... filenameN`；
  2. 选择本地的存储目录；
  3. 等待下载。

>PS：远端服务器也需要安装lrzsz。

## 五、参考
- [x] [https://github.com/mmastrac/iterm2-zmodem](https://github.com/mmastrac/iterm2-zmodem)






