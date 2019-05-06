---
layout: post
title: Linux小命令收集
date: 2018-11-23 11:24:00 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

### Linux小命令收集

#### 1.linux 删除不同文件夹内相同的文件

~~~shell
find A -name c.txt | xargs rm -rf
~~~

之所以用到这个命令，关键是由于很多命令不支持管道来传递参数，而日常工作中有这个必要，所以就有了xargs命令。



参考： <https://zhidao.baidu.com/question/258418012.html> 

#### 2.tar命令解压时如何去除目录结构及其解压到指定目录 (--strip-components N) 

```shell
tar zxvf fpga_package.tar.gz --strip-components 1  -C /root/junliang/
```
参考： <https://www.cnblogs.com/zhangmingcheng/p/7150888.html>
#### 3.grep同时搜索2个关键字

```shell
grep -E '(vmx|svm)' /proc/cpuinfo
```

#### 4.磁盘格式化或挂载报错

报错信息如下：

~~~shell
/dev/sda is apparently in use by the system; will not make a filesystem here!
mount: /dev/sda1 already mounted or /media/ busy 

~~~

处理方法:

~~~shell
#首先检查dm中记录信息 
    [root@localhost /]# dmsetup status 
    VolGroup-lv_swap: 0 4063232 linear 
    VolGroup-lv_root: 0 36847616 linear 
    sda3: 0 1044225 linear 
    sda1: 0 2120517 linear 
#移除sda1和3 
    [root@localhost /]# dmsetup remove sda1 
    [root@localhost /]# dmsetup remove sda3 
#重新mount或mkfs    
    
~~~

参考：https://www.cnblogs.com/xiaoxiangyucuo/p/5573662.html

#### 5.linuxOS关闭图形化

~~~shell
[root@bdw-ep2 ~]# runlevel
N 3
[root@bdw-ep2 ~]# systemctl get-default
graphical.target
[root@bdw-ep2 ~]# systemctl set-default multi-user.target
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/multi-user.target.
[root@bdw-ep2 ~]#

~~~

参考：https://www.linuxidc.com/Linux/2016-04/130558.htm



