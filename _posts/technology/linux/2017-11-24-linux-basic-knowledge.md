---
layout: post
title: linux基础知识
date: 2017-11-24 14:24:23 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

# 1.linux基础知识

## 1.1 目录介绍

### 1.1.1 管理类
- /bin 执行程序。
- /sbin 管理员执行程序。
- /dev 设备
- /etc 配置文件
- /boot 启动文件、内核
- /var 存放程序运行时信息，如系统日志、用户信息、缓存等。

### 1.1.2 用户类
- /root root目录
- /home 普通用户主目录存放地
- /usr 是系统核心所在，包含了所有的共享文件。它是 unix 系统中最重要的目录之一，涵盖了二进制文件，各种文档，各种头文件，x，还有各种库文件；还有诸多程序，例如 ftp，telnet 等等。<br>虽然看起来像user，但是跟里面没有用户文件。

里面存放这许多Linux系统文件。


### 1.1.3 信息类
- /proc 查询各种系统信息
- /lost-found 备份重要信息

### 1.1.3 应用类
- /opt 安装用户应用软件，内核日志
- /lib 存放供可执行程序使用的各种代码库。代码库分为静态库和共享库，/lib目录下一般只有共享库。

# 2.shell常用命令

### 2.1 时间和时区设置
	date -s 2017-4-12 [将系统日期设定成2017年4月12日的命令]
	date -s 10:35:00  [将系统时间设定成10点35分0秒的命令]
	[root@ccd-sdv6 ~]# ls /etc/localtime
	/etc/localtime
	[root@ccd-sdv6 ~]# ll /etc/localtime
	lrwxrwxrwx. 1 root root 38 Oct 16  2016 /etc/localtime -> ../usr/share/zoneinfo/America/New_York
	[root@ccd-sdv6 ~]#  mv /etc/localtime /etc/localtime.bak
	[root@ccd-sdv6 ~]# ln -s /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime 
	[root@ccd-sdv6 ~]# ll /etc/localtime
	lrwxrwxrwx 1 root root 33 Sep  7 02:41 /etc/localtime -> /usr/share/zoneinfo/Asia/Shanghai
	
	查看系统当前时间：
	[root@ccd-sdv6 ~]# date
	Thu Sep  7 02:41:20 CST 2017
	
	按照"年-月-日 小时：分钟：秒"的格式：
	[root@ccd-sdv3 ~]# date "+%Y-%m-%d %H:%M:%S"
	2017-11-17 14:58:04
	
	设置系统时间为2017年11月17日3点27分
	[root@ccd-sdv3 ~]# date -s "20171117 15:27:30"
	Fri Nov 17 15:27:30 CST 2017
	
	查看本地系统时区：
	[root@ccd-sdv3 ~]# date "+%Z"
	CST
	
	查看星期几：
	[root@ccd-sdv3 ~]# date "+%A"
	Friday
	
	查看当前是上午还是下午：
	[root@ccd-sdv3 ~]# date "+%p"
	PM
	
	判断今天是一年中的第几天：
	[root@ccd-sdv3 ~]# date "+%j"
	321
	
	[root@ccd-sdv6 ~]# date -s 2017-9-6
	Wed Sep  6 00:00:00 CST 2017
	[root@ccd-sdv6 ~]# date -s 14:42:22
	Wed Sep  6 14:42:22 CST 2017
	[root@ccd-sdv6 ~]# date
	Wed Sep  6 14:42:24 CST 2017
	[root@ccd-sdv6 ~]# hwclock --systohc [系统时间和硬件时间同步]
	[root@ccd-sdv6 ~]# hwclock
	Wed 06 Sep 2017 02:42:50 PM CST  -0.829138 seconds
	#注意：hwclock(clock) --hctosys是硬件时间与系统时间同步
	#sys代表系统时间，hc代表硬件时间

### 2.2 cat tac rev的区别：
cat是显示文件夹的命令，全称是concatenate，tac是cat的倒写，意思也和它是相反的。cat是从第一行显示到最后一行，而tac是从最后一行显示到第一行，而rev 则是从最后一个字符显示到第一个字符.

```shell
[root@localhost ~]# cat test
abcd
123456
[root@localhost ~]# tac test
123456
abcd
[root@localhost ~]# rev test
dcba
654321
[root@localhost ~]#
```

### 2.3 查看系统的挂载信息及磁盘空间使用情况：

mount |column -t 用一个很不错的格式与规范列出所有挂载文件系统。

```shell
[root@ccd-sdv9 ~]# mount |column -t
/dev/nvme0n1p2                              on  /boot                                               type  xfs              (rw,relatime,attr2,inode64,noquota)
/dev/nvme0n1p1                              on  /boot/efi                                           type  vfat             (rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=ascii,shortname=winnt,errors=remount-ro)
192.168.199.52:/share/images                on  /share/xvs/img                                      type  nfs4             (rw,relatime,vers=4.0,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=192.168.199.169,local_lock=none,addr=192.168.199.52)
192.168.199.52:/share/tools                 on  /share/xvs/tools                                    type  nfs4             (rw,relatime,vers=4.0,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=192.168.199.169,local_lock=none,addr=192.168.199.52)
192.168.199.52:/share/log/ccd-sdv9/results  on  /share/xvs/results                                  type  nfs4             (rw,relatime,vers=4.0,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=192.168.199.169,local_lock=none,addr=192.168.199.52)
tmpfs                                       on  /run/user/1000                                      type  tmpfs            (rw,nosuid,nodev,relatime,size=3200100k,mode=700,uid=1000,gid=1000)
gvfsd-fuse                                  on  /run/user/1000/gvfs                                 type  fuse.gvfsd-fuse  (rw,nosuid,nodev,relatime,user_id=1000,group_id=1000)
/dev/sda2                                   on  /run/media/m2/82ef97ec-68ee-46f5-9709-90aa2d995ded  type  xfs              (rw,nosuid,nodev,relatime,attr2,inode64,noquota,uhelper=udisks2)
tmpfs                                       on  /run/user/0                                         type  tmpfs            (rw,nosuid,nodev,relatime,size=3200100k,mode=700)
```


```shell
[root@ccd-sdv9 ~]# df -h
Filesystem                                  Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root                       111G   45G   60G  43% /
devtmpfs                                     16G     0   16G   0% /dev
tmpfs                                        16G  100K   16G   1% /dev/shm
tmpfs                                        16G   11M   16G   1% /run
tmpfs                                        16G     0   16G   0% /sys/fs/cgroup
/dev/nvme0n1p2                             1014M  340M  675M  34% /boot
/dev/nvme0n1p1                              200M  9.5M  191M   5% /boot/efi
192.168.199.52:/share/images                2.0T  857G  1.1T  45% /share/xvs/img
192.168.199.52:/share/tools                 246G  6.9G  227G   3% /share/xvs/tools
192.168.199.52:/share/log/ccd-sdv9/results  8.0T  2.5T  5.2T  32% /share/xvs/results
tmpfs                                       3.1G   12K  3.1G   1% /run/user/1000
/dev/sda2                                  1014M   41M  974M   5% /run/media/m2/82ef97ec-68ee-46f5-9709-90aa2d995ded
tmpfs                                       3.1G     0  3.1G   0% /run/user/0
[root@ccd-sdv9 ~]#
```

**扩展与延伸：**<br>
若想开机自动挂载，只要修改/etc/fstab文件即可。<br>
文件挂载的配置文件：/etc/fstab
	[root@ccd-sdv9 ~]# cat /etc/fstab
	

```shell
#
# /etc/fstab
# Created by anaconda on Fri Dec 30 16:54:07 2016
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/rhel-root   /                       ext4    defaults        1 1
UUID=f76e5df9-fa2a-4800-9f13-8312eba36b65 /boot                   xfs     defaults        0 0
UUID=77B3-50A7          /boot/efi               vfat    umask=0077,shortname=winnt 0 0
/dev/mapper/rhel-swap   swap                    swap    defaults        0 0
```

每行定义一个要挂载的文件系统；

其每行的格式如下:

要挂载的设备或伪文件系统  挂载点  文件系统类型  挂载选项 转储频率 自检次序<br>
dev/mapper/rhel-root   /                       ext4    defaults        1 1

要挂载的设备或伪文件系统：设备文件、LABEL(LABEL="")、UUID(UUID="")、伪文件系统名称(proc, sysfs)

挂载点：指定的文件夹

挂载选项：defaults

转储频率：

0：不做备份

1：每天转储

2：每隔一天转储

自检次序：

0：不自检

1：首先自检；一般只有rootfs才用1；


# 3.其它

```shell
1)find ./ -name  '*' | grep "qemu" 

2)xargs 将标准输入转换成命令行参数
例如：
linux 删除不同文件夹内相同的文件
	find A -name c.txt | xargs rm -rf
	#之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了xargs命令

3)2>&1 > /dev/null
	#不让信息显示在屏幕，不论正确与否，都输出到/dev/null

4)uptime 命令用于查看系统的负载情况
```
我经常会用"watch -n 1 uptime"来每秒刷新一次获得当前系统的负载情况，输出内容分别为系统当前时间、系统已运行时间、当前在线用户以及平均负载值。<br>而平均负载分为最近1分钟、5分钟、15分钟的系统负载情况，负载值越低越好(小于1是正常)。

```shell
[root@ccd-sdv3 ~]# uptime
 14:49:43 up  1:08,  5 users,  load average: 2.56, 2.33, 2.05
[root@ccd-sdv3 ~]#
```

 
