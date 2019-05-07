# LessOrMore


致谢
====================================
+ 感谢[jekyll主题官网](http://jekyllthemes.org/themes/Less-Or-More/)的样式，本Jekyll框架的样式都是从jekyll主题官网的样式直接拷贝过来的。只是重构了JS，并且加入了Jekyll语法而已。
+ 感谢[Github](https://github.com/)提供的代码维护和发布平台
+ 感谢[Jekyll](https://jekyllrb.com/)团队做出如此优秀的产品



使用
====================================

下载
------------------------------------

使用git从[LessOrMore](https://github.com/luoyan35714/LessOrMore.git)主页下载项目

``` bash
git clone https://github.com/luoyan35714/LessOrMore.git
```

配置
------------------------------------

`LessOrMore`项目需要配置的只有一个文件`_config.yml`，打开之后按照如下进行配置。

> 特别注意`baseurl`的配置。如果是`***.github.io`项目，不修改为空''的话，会导致JS,CSS等静态资源无法找到的错误

``` bash
name: 博客名称
email: 邮箱地址
author: 作者名
url: 个人网站
# baseurl修改为项目名，如果项目是'***.github.io'，则设置为空''
baseurl: "/LessOrMore"
resume_site: 个人简历网站
github: github地址
github_username: github用户名称
```

关于统计
------------------------------------

本项目支持三种统计，分别是

+ [百度统计](https://tongji.baidu.com)

百度统计是后台统计，并没有再页面有任何展示，但是可以通过登录百度统计官网查看更详细的访问记录分析。
当Fork本项目之后需要去百度统计官网申请自己的baidu统计ID替换 `_config.yml` 文件中的 `baidu_analysis`。

+ [revolvermaps地图统计](http://www.revolvermaps.com/)

revolvermaps地图统计是展示在左侧当行栏的地图展示，具体展示形式可以去官网定制。
当Fork本项目之后需要去revolvermaps地图官网申请自己的统计ID， 替换`_config.yml` 文件中的 `revolvermaps`。

+ [不蒜子统计](http://busuanzi.ibruce.info/)

不蒜子统计是出现在页面右上角的`本站总访问量n次`统计，本统计会自动识别客户所对应的域名，根据不同域名统计，所以并不需要Fork本项目者做任何修改。
更多不蒜子的展示方式可以去官网查看。


如何写文章
------------------------------------

在`LessOrMore/_posts`目录下新建一个文件，可以创建文件夹并在文件夹中添加文件，方便维护。在新建文件中粘贴如下信息，并修改以下的`titile`,`date`,`categories`,`tag`的相关信息，添加`* content {:toc}`为目录相关信息，在进行正文书写前需要在目录和正文之间输入至少2行空行。然后按照正常的Markdown语法书写正文。

``` bash
---
layout: post
#标题配置
title:  标题
#时间配置
date:   2016-08-27 01:08:00 +0800
#大类配置
categories: document
#小类配置
tag: 教程
---

* content
{:toc}


我是正文。我是正文。我是正文。我是正文。
```

效果
------------------------------------
打开浏览器并输入URL`https://hjl21.github.io/`,回车。


选主题的经历
====================================

很明显，我在重复造轮子。在16年接触到GIT，17年下半年接触到Jekyll，然后搭建了自己的博客，当时就用jekyll自带的风格格式一直到现在。随着文章的增加，就感觉该页面风格比较单调，阅读不方便。并且不区分一级标题和二级标题，导致没有重点强调。诸如此类，用了2年，用的越多，越发吃力，于是就一直在寻找新的能够让我一眼认定的主题。

虽然jekyll官网上的主题很多。但是真正适合拿来做博客的却不多。直到有一天看到LessOrMore这个主题之后，豁然觉得这就是我的博客想要的样子。简单而又不平凡。所以就决定了要把博客迁移到这个主题，然后大概零零散散花费一周的时间把这个主题做出来。

迄今为止自己觉得最适合我的博客，所以是值得的！

关于作者
====================================

热爱技术，热爱折腾的IT爱好者。

写在最后
====================================

如果你也像我一样在寻觅一个简洁的博客主题。不妨试下LessOrMore。
