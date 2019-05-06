---
layout: post
title: linux java环境变量设置
date: 2018-11-22 10:24:23 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

# linux java环境变量设置

先下载相应的jre包，并解压

在/root/.bashrc文件里面加入以下3行就可以了

```shell
export JRE_HOME=/root/jre1.8.0_181
export PATH=$JRE_HOME/bin:$PATH
export CLASSPATH=.:$JRE_HOME/lib:$CLASSPATH
```

最后:
source /root/.bashrc 使其生效

