---
layout: post
title: YUM源配置
date: 2017-11-01 10:24:23 +8000
categories: 技术文档
tag: Linux
---

* content

{:toc}

# YUM源配置

## 1.redhat

### 1.1配置安装源 redhat

```shell
配置 /etc/yum.repo.d/ 源
$ cat linux-ftp.repo
 [rhel$releasever]
 name=Red Hat Enterprise Linux $releasever
 baseurl=http://linux-ftp.sh.intel.com/pub/ISO/redhat/redhat-rhel/RHEL-7.3-Snapshot-4/Server/x86_64/os/
 enabled=1
 gpgcheck=0

[rhel6_optional]
 name=Red Hat Enterprise Linux rhel6_optional
 baseurl=http://linux-ftp.sh.intel.com/pub/ISO/redhat/redhat-rhel/RHEL-7.3-Snapshot-4/Server-optional/x86_64/os/
 enabled=1
 gpgcheck=0

执行 yum update
```
### 1.2配置本地yum源

	本文配置本地yum源是把RedHat 7的系统盘内容复制到服务器硬盘的目录/RH7ISO中，然后配置yum指向该目录。
- 首先挂载光驱到/mnt目录 ：mount /dev/cdrom /mnt
- 复制系统盘的内容到/rhel7iso目录中：cp -R /mnt/* rhel7iso
- 进入yum配置目录 : cd /etc/yum.repos.d/
- 建立yum配置文件: touch rhel7_iso.repo

•编辑配置文件，添加以下内容: vim rhel7_iso.repo

```shell
[RHEL7ISO] name=rhel7iso
 baseurl=file:///rhel7iso
 enabled=1
 gpgcheck=1
 gpgkey=file:///rhel7iso/RPM-GPG-KEY-redhat-release
```

+ 清除yum缓存: yum clean all
+ 缓存本地yum源中的软件包信息: yum makecache

+ 配置完毕！可以直接使用yum install packname进行yum安装了！

## 2.Ubuntu

### 2.1配置安装源 ubuntu

	请注意： 如果在安装中部分软件无法安装成功，说明软件源中缺包，先尝试使用命令  
	#apt-get update更新软件源后尝试安装。如果还是不行，需要更换软件源。更换步骤：
	输入命令#cp /etc/apt/sources.list /etc/apt/sources.list_backup
	输入命令#vi /etc/apt/sources.list
	添加其他软件源（推荐使用163、中科大、上海交大等速度较快的国内源）
	保存并关闭窗口
	输入命令：#apt-get update
### 2.2安装远程源

```shell
#for ubuntu14.04.4 source

gedit /etc/apt/sources.list

deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty main restricted
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty main restricted
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates main restricted
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates main restricted
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty universe
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty universe
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates universe
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates universe
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty multiverse
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty multiverse
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates multiverse
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-updates multiverse
deb http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://linux-ftp.sh.intel.com/pub/mirrors/ubuntu/ trusty-backports main restricted universe multiverse
```


### 2.3安装本地源

	第一步转到镜像的下载目录，挂载ISO镜像挂载至/media/cdrom下。  
	代码: sudo mount -o loop -t iso9660 update-i386-20080312-CD1.iso /media/cdrom
	
	第二步手动添加ISO镜像至软件源列表，这样就可以在软件库里找到ISO上所有的软件包。  
	代码: sudo apt-cdrom -m -d=/media/cdrom add
	
	第三步刷新软件库 代码: sudo apt-get update
	
	注意，执行完成后查看/etc/apt/sources.list文件，确保文件如下一行在文件顶部或者在网络源前面，  
	否者，安装软件的时候系统还是优先从网络上下载【建议把除了dvd本地源之外的下面所有项注视掉，不建议删除，  
	之后在apt-get update更新下】   
	deb cdrom:[Ubuntu 9.04 Jaunty Jackalope - Release i386 (20090421.3)]/ jaunty main restricted
	
	之后就可以用apt-get install ** 来安装软件包了，不过有点问题，
	这命令执行一次可能会不成功，多执行几次就OK了

## sles 

### 安装本地源

```
1)下载与OS相关的iso文件(一般有2个如:SLE-12-SP2-SDK-DVD-x86_64-GM-DVD1.iso,  
SLE-12-SP2-Server-DVD-x86_64-GM-DVD1.iso)	
2)运行yast2,把2个iso文件添加进去即可
#如何添加不上，可以用mount
如：mount SLE-12-SP1-SDK-DVD-x86_64-Beta2-DVD1.iso /test
cd /test/suse/x86_64
#注意sles安装软件，用zypper命令
如:zypper install python-devel
```

