---
title: 诞生记——GitHub Pages+Hexo
date: 2017-04-01 11:20:22
tags: [Hexo]
categories: [前端]
description: GitHub Pages,Hexo,Git,静态博客
---
## 为什么要使用静态博客？
- 轻量级的博客系统，没有麻烦的配置，跨平台同步文件；
- 无需自己搭建服务器；
- 使用标记语言，如[Markdown](http://markdown.tw/)；
- 根据Github的限制，对应的每个站有300MB空间；
- 可以绑定自己的域名。

> [静态站点生成器列表](https://staticsitegenerators.net/)

## 环境准备

1. [Node.js官网](https://nodejs.org/)下载相应平台（注：此篇为*Windows*）的[最新版本](https://nodejs.org/en/download/)安装。
2. Git使用msysgit，[下载安装](https://git-scm.com/download/)。
3. 注册[GitHub](https://github.com)账号，创建相应仓库，添加SSH公钥。
  > 注意：[个人（组织）主页与项目主页](https://help.github.com/articles/user-organization-and-project-pages/)区别！[本博客](http://blog.wuzhiwei.cn)部署在项目主页（Project Pages）的*gh-pages*分支下，并[绑定二级子域](https://help.github.com/articles/setting-up-a-custom-subdomain/)*blog.wuzhiwei.cn*。

## Hexo
[Hexo官网](https://hexo.io/)，[中文文档传送门](https://hexo.io/zh-cn/docs/)。

### 安装
*Node.js*和*Git*安装好后，可执行下面命令安装*Hexo*：
``` bash
npm install hexo-cli -g
```
### 初始化
*cd*到目标目录（/path/to/hexo），执行下面命令进行初始化：
``` bash
hexo init #或 hexo init </path/to/hexo>
```
### 目录介绍
初始化完成后，会产生如下目录：
``` bash
.
├── .deploy_git
├── public # 此目录是稍后，hexo generate生成的
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
├── themes
├── _config.yml
└── package.json
```
- *.deploy_git*：执行`hexo deploy`命令，部署到*GitHub*上的内容目录；
- *public*：执行`hexo generate`命令，生成的静态网页内容目录；
- *scaffolds*：*layout*模板文件目录，其中的*md*文件可以添加编辑；
- *source*：文章源码目录，此目录下的*md*和*html*文件都会被Hexo处理；该目录对应*repo*的根目录，404文件、favicon.ico文件、CNAME文件都应该放这里；该目录下可以新建页面目录：
  - *_drafts*：草稿文章；
  - *_posts*：发布文章；
- *themes*：主题文件目录；
- *_config.yml*：全局配置文件；
- *package.json*：应用程序数据，指明Hexo的版本等信息。

### 常用命令
``` bash
hexo generate # 生成静态文件，会在当前目录下生成一个pulic文件夹，可简写为：hexo g
hexo server # 启动本地WEB服务，用于博客的预览，可简写为：hexo s
hexo deploy # 部署到远端（如GitHub等），可简写为：hexo n

hexo new "postName" # 新建文章
hexo new page "pageName" # 新建页面
# 组合使用
hexo d -g # 生成部署
hexo s -g # 生成预览
```
更多命令可参阅[Hexo帮助文档](https://hexo.io/zh-cn/docs/)。

### 主题设置
**安装主题**
``` bash
hexo clean
git clone https://github.com/path/to/theme.git themes/theme
```

**启用主题**
修改Hexo目录下的*_config.yml*配置文件中的*theme*属性，指定对应的主题名即可。

**更新主题**
``` bash
cd themes/theme
git pull
hexo s -g
```

主题目录示例：
``` bash
.
├── languages          #多语言
|   ├── default.yml    #默认语言
|   └── zh-CN.yml      #中文语言
├── layout             #布局，根目录下的*.ejs文件是对主页，分页，存档等的控制
|   ├── _partial       #局部的布局，此目录下的*.ejs是对头尾等局部的控制
|   └── _widget        #小挂件的布局，页面下方小挂件的控制
├── source             #源码
|   ├── css            #css源码 
|   |   ├── _base      #*.styl基础css
|   |   ├── _partial   #*.styl局部css
|   |   ├── fonts      #字体
|   |   ├── images     #图片
|   |   └── style.styl #*.styl引入需要的css源码
|   ├── fancybox       #fancybox效果源码
|   └── js             #javascript源代码
├── _config.yml        #主题配置文件
└── README.md          #用GitHub的都知道
```

### 404页面
制作404页面，直接在*/source/*目录下创建自己的*404.html*或*404.md*文件即可。GitHub Pages有提供制作404页面的指引：[Custom 404 Pages](https://help.github.com/articles/custom-404-pages)。

**推荐**使用[腾讯公益404](http://www.qq.com/404/)。

## 附A：参考
- [x] [將Hexo部署到GitHub Page](http://alincode.github.io/blog/2016/03/05/hexo-deploy/)
- [x] [GitHub Pages指南](http://wiki.jikexueyuan.com/project/github-pages-basics/)
- [x] [Hexo在github上构建免费的Web应用](http://blog.fens.me/hexo-blog-github/)

<!-- more -->
## 附B：资源地址
- [Webluker-CDN网站加速](http://www.webluker.com/)
- [监控宝-网站监控](http://www.jiankongbao.com/)
- [谷歌站长工具](http://www.google.com/intl/zh-CN/webmasters)
- [百度站长工具](http://zhanzhang.baidu.com/)
- [站长之家工具](http://tool.chinaz.com/)
- [360搜索站长平台](http://zhanzhang.so.com/)
- [360网站安全检测](http://webscan.360.cn/)
- [360云监控](http://jk.cloud.360.cn/)
- [不蒜子](http://busuanzi.ibruce.info/)
- [百度统计](http://tongji.baidu.com/)
- [Google Analytics](http://www.google.com/analytics/web/?hl=zh-CN)
- [腾讯企业邮箱](http://exmail.qq.com/)
- [邮箱地址生成图片](http://pic.sdodo.com/tool/mailpic)
- [MakePic.com邮址图片生成](http://www.makepic.com/email.php)
- [Favicon制作](http://tool.lu/favicon)
- [无觅关联推荐](http://www.wumii.com/widget/relatedItems)
- [友荐](http://www.ujian.cc/)
- [百度推荐](http://tuijian.baidu.com/)
- [百度联盟](http://union.baidu.com/)
- [StackEdit](https://stackedit.io/)
- [HexoThemes](https://github.com/hexojs/hexo/wiki/Themes)
- [DNSPod](https://www.dnspod.cn/)
- [搜索引擎提交入口](http://www.sousuoyinqingtijiao.com/)
- [支付宝二维码](https://qr.alipay.com/paipai/open.htm)

## 附C：参考主题
- [fexo](https://github.com/forsigner/fexo) | [Demo](http://forsigner.com/)
- [Vno](https://github.com/lenbo-ma/hexo-theme-vno) | [Demo](http://mlongbo.com/)
- [Alberta](https://github.com/ken8203/hexo-theme-alberta) | [Demo](http://jaychung.tw/)
- [Noderce](https://github.com/willerce/hexo-theme-noderce) | [Demo](http://willerce.com/)
- [Maupassant](https://github.com/tufu9441/maupassant-hexo) | [Demo](https://www.haomwei.com/)
- [Maupassant](https://github.com/7ye/maupassant-hexo) | [Demo](http://sevennight.cc/)
- [NexT](https://github.com/iissnan/hexo-theme-next) | [Demo](http://heroicyang.com)
- [Mala](https://github.com/idhyt/hexo-theme-next/tree/magiclamp) | [Demo](http://blog.idhyt.com/)
- [Huno](https://github.com/someus/huno) | [Demo](http://letiantian.me/)
- [Air](https://github.com/hustcer/hexo-theme-air) | [Demo](http://topdna.org/)
- [Aloha](https://github.com/henryhuang/hexo-theme-aloha) | [Demo](http://huangyijie.com/)
- [Athena](https://github.com/steven5538/hexo-theme-athena) | [Demo](http://steven5538.tw/)
- [Huno](https://github.com/someus/huno) | [Demo](http://letiantian.me/)
- [Iceman](https://github.com/wizicer/iceman) | [Demo](http://icerdesign.com/)
- [NexT](https://github.com/iissnan/hexo-theme-next) | [Demo](http://notes.iissnan.com/)
- [Oishi](https://github.com/henryhuang/oishi) | [Demo](http://henryhuang.github.io/oishi/)
- [TKL](https://github.com/SuperKieran/TKL) | [Demo](http://go.kieran.top/)
- [Yilia](https://github.com/litten/hexo-theme-yilia) | [Demo](http://litten.me/)
- [DrakeLeung](https://github.com/DrakeLeung/blog) | [Demo](https://lyyourc.com)
- [Anatole](https://github.com/Ben02/hexo-theme-Anatole) | [Demo](http://anatole.munen.cc)
- [Apollo](https://github.com/pinggod/hexo-theme-apollo) | [Demo](http://pinggod.com)
- [Hacker](https://github.com/CodeDaraW/Hacker) | [Demo](https://blog.daraw.cn/)
- [Random](https://github.com/stiekel/hexo-theme-random) | [Demo](https://chensd.com/)
- [Typing](https://github.com/geekplux/hexo-theme-typing) | [Demo](http://geekplux.com)
- [Yelee](https://github.com/MOxFIVE/hexo-theme-yelee) | [Demo](http://moxfive.xyz)
- [Carbon](https://github.com/icylogic/carbon) | [Demo](https://icylogic.github.io/carbon/)