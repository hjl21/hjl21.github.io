---
layout: post
title: fatal error:xxx.h no such file or directory 解决方案
date: 2018-10-24 12:04:00 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

### fatal error:xxx.h no such file or directory 解决方案 ###

编译程序的时候遇到了找不到依赖包的问题，找了很多解决方案都没有解决，最后摸索了很久才解决，把过程记录下来方便以后用。
碰到这种问题，首先是要找有没有可安装的依赖包，命令为：

```shell
apt-cache search xxx | grep dev
```

用dev过滤是因为有时候相关的安装包有很多，所以过滤一下

过滤之后直接安装给出的包就好

如果能无错下载一般就没有后面的问题了，但是我安装的时候就碰到了各种问题，比如

The following packages have unmet dependencies:
 lib***-dev : Depends: lib*** (= 5.5.35+dfsg-1ubuntu1) but 5.5.44-0ubuntu0.14.04.1 is to be installed

或者
E: Unable to correct problems, you have held broken packages.

这个时候就很纠结了，因为很多安装包的依赖关系很复杂，不那么容易移除后自己重装

这个时候就有一个很好用的工具了:aptitude<br>
“aptitude与 apt-get 一样，是 Debian 及其衍生系统中功能极其强大的包管理工具。<br>
与 apt-get 不同的是，aptitude在处理依赖问题上更佳一些。举例来说，aptitude在删除一个包时，会同时删除本身所依赖的包。<br>这样，系统中不会残留无用的包，整个系统更为干净。"


装好aptitude之后，可以试一下命令sudo aptitude update && sudo aptitude install build-essential

之后编译，若还不能过，可以用aptitude安装来试一下，仔细看下aptitude给的选项和提示，就可以方便的修复上面的问题了

依赖包安装好之后，我又碰到了/usr/bin/ld:cannot find -lxxx这样的问题.<br>
如果依赖包已经正确的安装，出现这个问题很可能是因为链接文件没有设置好,<br>
这个时候需要cd 到/usr/lib命令下，先用ls+grep把本来的链接库找出来<br>
然后使用ln命令把lxxx文件和相应的链接库链接


经过以上步骤，这个问题应该就能解决了

### 参考文件 ###

[https://blog.csdn.net/cocosola/article/details/50336925]()
