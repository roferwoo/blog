---
title: Nginx笔记
date: 2017-04-22 09:40:32
categories: [Nginx]
description: Nginx,Web服务器,Mac开发,博客,http://blog.wuzhiwei.cn,http://wuzhiwei.cn
---
## 简介
> [官网](http://nginx.org/)

**Nginx**（"engine x"）是一个高性能、轻量级的Web服务器/反向代理服务器及电子邮件（IMAP/POP3/SMTP）服务器。Nginx是由*Igor Sysoev*为俄罗斯访问量第二的Rambler.ru站点开发的。其特点是**占有内存少、并发能力强**。

> 第一个公开版本0.1.0发布于2004年10月4日。将源代码以类BSD许可证的形式发布。

## 安装
### Windows
[下载](http://nginx.org/en/download.html)最新稳定版本，解压至相应目录即可**（绿色文件，无须安装）**。安装完成后，可通过以下方式启动：

1. 双击*nginx.exe*运行，可见黑窗口一闪而过，启动完毕；
2. 命令行到*nginx*目录，输入`nginx -c conf/nginx.conf`*（可指定配置文件）*启动。
 > 注：此方式命令行窗口无任何提示，且**被锁定**。
3. 命令行到*nginx*目录，输入`start nginx`启动，此方式**不锁定**。

关闭Nginx服务：
```
/path/to/nginx > nginx -s stop （或在任何目录下执行：taskkill /F /IM nginx.exe > nul）
```
查看Nginx进程：
```
tasklist /fi "imagename eq nginx.exe"
```

> `nginx -V`可查看编译版本支持哪些模块。

### Mac
通过*Homebrew*安装，在命令行终端执行：
```
brew install nginx
```

## 常用命令
```
nginx -s stop # 强制关闭
nginx -s quit # 安全关闭
nginx -s reload # 改变配置文件的时候，重启nginx工作进程，来使配置文件生效
nginx -s reopen # 打开日志文件
nginx -t # 测试配置是否正确
```

## *location*匹配规则

### *location*匹配命令

- `~`：波浪线表示执行一个正则匹配，*区分大小写*。
- `~*`：表示执行一个正则匹配，*不区分大小写*。
- `^~`：表示普通字符匹配，如果该选项匹配，只匹配该选项，不匹配别的选项，一般用来匹配目录。
- `=`：进行普通字符精确匹配。
- `@`：定义一个命名的*location*，使用在内部定向时，例如*error_page*、*try_files*。

### *location*匹配的优先级
> 优先级与*location*在配置文件中的顺序无关。

`=`*精确匹配*会第一个被处理。如果发现精确匹配，nginx停止搜索其他匹配。

*普通字符匹配*、*正则表达式规则*和*长的块规则*将被优先和查询匹配，也就是说如果该项匹配还需去看有没有正则表达式匹配和更长的匹配。
`^~`则只匹配该规则，nginx停止搜索其他匹配，否则nginx会继续处理其他*location*指令。

最后匹配理带有`~`和`~*`的指令，如果找到相应的匹配，则nginx停止搜索其他匹配；当没有正则表达式或者没有正则表达式被匹配的情况下，那么匹配程度最高的逐字匹配指令会被使用。

例如：
```
location  = / {
  # 只匹配"/"
  [ configuration A ]
}
location  / {
  # 匹配任何请求，因为所有请求都是以"/"开始
  # 但是更长字符匹配或者正则表达式匹配会优先匹配
  [ configuration B ]
}
location ^~ /images/ {
  # 匹配任何以 /images/ 开始的请求，并停止匹配 其它location
  [ configuration C ]
}
location ~* .(gif|jpg|jpeg)$ {
  # 匹配以 gif, jpg, or jpeg结尾的请求
  # 但是所有 /images/ 目录的请求将由 [Configuration C]处理
  [ configuration D ]
}

# 请求URI示例
# / -> 符合configuration A
# /documents/document.html -> 符合configuration B
# /images/1.gif -> 符合configuration C
# /documents/1.jpg ->符合 configuration D

# @ location例子
error_page 404 = @fetch;

location @fetch(
  proxy_pass http://fetch;
)
```

## Nginx+PHP
Apache一般把PHP当做自己的一个模块来启动；而Nginx则是把HTTP请求变量（如*get*、*user_agent*等）转发给PHP进程，即PHP独立进程与Nginx进行通信，称为**fastcgi**运行方式。

### Windows

[下载最新版PHP](http://windows.php.net/download/)，解压，重命名*php.ini-development*为*php.ini*，修改配置：
```
# 指定win7下的扩展目录。
extension_dir = "./ext"
# 允许用户在运行时加载PHP扩展，即在脚本运行期间加载。
enable_dl = On
# 启用时, 防止任何人通过如http://my.host/cgi-bin/php/secretdir/script.php这样的URL直接调用PHP。PHP在此模式下只会解析已经通过了web服务器的重定向规则的URL。在大多数web服务器中以CGI方式运行PHP时很有必要用cgi.force_redirect提供安全。PHP默认其为On。可以将其关闭，但风险自担。注: Windows用户，可以安全地在IIS之下将其关闭，事实上必须这么做。要在OmniHTTPD或Xitami之下使用也必须将其关闭。
cgi.force_redirect = 0
# 1:PHP CGI以/为分隔符号从后向前依次检查请求的路径, 对CGI提供了真正的PATH_INFO/PATH_TRANSLATED支持。以前PHP的行为是将PATH_TRANSLATED设为SCRIPT_FILENAME，而不管PATH_INFO是什么。有关PATH_INFO的更多信息见cgi规格。将此值设为1将使PHP CGI修正其路径以遵守规格。设为0 将使PHP的行为和从前一样。默认为零。用户应该修正其脚本使用SCRIPT_FILENAME而不是PATH_TRANSLATED。
cgi.fix_pathinfo = 1
# IIS（在基于 WINNT 的操作系统上）中的FastCGI支持模仿客户端安全令牌的能力。这使得IIS能够定义运行时所基于的请求的安全上下文。Apache中的mod_fastcgi不支持此特性（03/17/2002）。如果在IIS 中运行则设为1。默认为 0。
fastcgi.impersonate = 1
# 指定PHP在发送HTTP响应代码时使用何种报头。如果设定为0，PHP发送一个Status: 报头，Apache和其它web server都支持。如果此选项设定为1，PHP将发送RFC 2616兼容的报头。除非你知道自己在做什么，否则保留其值为 0。
cgi.rfc2616_headers = 1
date.timezone = Asia/ChongQing
```

启动PHP-FPM：`php-cgi.exe -b 127.0.0.1:9000`。

停止PHP-FPM：`taskkill /fi "imagename eq php-cgi.exe"`或`taskkill /F /IM php-cgi.exe > nul`。

修改Nginx配置文件以支持PHP-FPM比较简单，核心就一句话——*把请求的信息转发给9000端口的PHP进程，让PHP进程处理指定目录下的PHP文件*。

```
location ~ \.php$ {
    #root html; # 提取到server块中
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

## 附：nginx.conf
```
#user  nobody; #运行用户
worker_processes  1; #启动进程，通常设置成和cpu的数量相等

#全局错误日志 定义类型[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid; #进程文件 PID文件

#指定一个nginx进程可以打开的最多文件描述符数目
worker_rlimit_nofile 51200;

#工作模式及连接数上限
events {
    #参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ];
    #epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
    #use   epoll;

    #单个后台worker process进程的最大并发连接数（最大连接数=连接数*进程数）
    worker_connections  1024;
}

#设定http服务器
http {
    include       mime.types; #文件扩展名与文件类型映射表，由mime.type文件定义
    default_type  application/octet-stream; #默认文件类型
    #charset utf-8; #默认编码

    #server_names_hash_bucket_size 128; #服务器名字的hash表大小
    #client_header_buffer_size 32k; #上传文件大小限制
    #large_client_header_buffers 4 64k; #设定请求缓
    #client_max_body_size 8m; #设定请求缓

    #设定日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，
    #对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。
    #注意：如果图片显示不正常把这个改 成off。
    sendfile        on;
    #autoindex on; #开启目录列表访问，适合下载服务器，默认关闭。
    #tcp_nopush     on; #防止网络阻塞
    #tcp_nodelay on; #防止网络阻塞

    #长连接超时时间，单位是秒
    #keepalive_timeout  0;
    keepalive_timeout  65;

    #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
    #fastcgi_connect_timeout 300;
    #fastcgi_send_timeout 300;
    #fastcgi_read_timeout 300;
    #fastcgi_buffer_size 64k;
    #fastcgi_buffers 4 64k;
    #fastcgi_busy_buffers_size 128k;
    #fastcgi_temp_file_write_size 128k;

    #gzip模块设置
    #gzip  on; #开启gzip压缩

    #gzip_min_length 1k; #最小压缩文件大小
    #gzip_buffers 4 16k; #压缩缓冲区
    #gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    #gzip_comp_level 2; #压缩等级
    #gzip_types text/plain application/x-javascript text/css application/xml; #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    #gzip_vary on;
    #limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用

    #负载均衡设置
    #upstream www.example.com {
    #upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
    #    server 192.168.80.121:80 weight=3;
    #    server 192.168.80.122:80 weight=2;
    #    server 192.168.80.123:80 weight=3;
    #}

    #设定虚拟主机配置
    server {
        listen       80;#侦听80端口
        server_name  localhost;#域名可以有多个，用空格隔开

        #charset koi8-r;#默认编码

        #access_log  logs/host.access.log  main;#设定本虚拟主机的访问日志

        #默认请求
        location / {
            root   html;#定义服务器的默认网站根目录位置
            index  index.html index.htm;#定义首页索引文件的名称
        }

        #图片缓存时间设置
        location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$ {
            expires 10d;
        }
        #JS和CSS缓存时间设置
        location ~ .*.(js|css)?$ {
            expires 1h;
        }

        #error_page  404              /404.html;# 定义错误提示页面

        #重定向服务器错误页面到静态页面/50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        #代理PHP脚本到Apache监听的127.0.0.1:80
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        #PHP脚本请求全部转发到FastCGI处理，使用FastCGI默认配置。
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        #拒绝访问.htaccess文件，如果Apache的文档根与nginx的一致
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    #另一个虚拟主机使用基于IP，基于名称和基于端口的配置的混合
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    #HTTPS服务器
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

## 参考
[x] [Nginx中文手册](http://www.nginx.cn)
