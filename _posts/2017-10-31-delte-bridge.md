---
layout: post
title: "删掉网桥"
date: 2017-10-31 13:19:37 +0800
categories: 技术文档
tag: Virtualiztion
---



[TOC]

# 删除网桥

```shell
[root@knl-f2 network-scripts]# brctl delif virbr0 virbr0-nic
[root@knl-f2 network-scripts]# brctl delbr virbr0
bridge virbr0 is still up; can't delete it
[root@knl-f2 network-scripts]# ifconfig virbr0 down
[root@knl-f2 network-scripts]# brctl delbr virbr0
[root@knl-f2 network-scripts]#
```

