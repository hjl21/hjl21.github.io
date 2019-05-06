---
layout: post
title: 查看linux发行版名称和版本号的8种方法
date: 2018-04-24 10:20:23 +8000
categories: 技术文档
tag: Linux
---



* content


{:toc}

# 查看linux发行版名称和版本号的8种方法

### 1.方法总览

- lsb_release 命令
- /etc/*-release 文件
- uname 命令
- /proc/version 文件
- dmesg 命令
- YUM 或 DNF 命令
- RPM 命令
- APT-GET 命令


### 2.方法详解

方法1： lsb_release 命令<br>
LSB (Linux标准库 Linux Standard Base)能够打印发行版的具体信息，包括发行版名称、版本号、代号等。<br>
需要安装相关的lsb包，如redhat-lsb

```shell
[root@ccd-sdv4 ~]# lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: RedHatEnterpriseServer
Description:    Red Hat Enterprise Linux Server release 7.3 (Maipo)
Release:        7.3
Codename:       Maipo
[root@ccd-sdv4 ~]#
```

方法2： /etc/*-release 文件<br>
release 文件通常被视为操作系统的标识。在/etc目录下放置了很多记录着发行版各种信息的文件，每个发行版都各自有一套这样记录着相关信息的文件。下面是一组在Redhat/Centos系统上显示出来的文件内容。

```shell
[root@ccd-sdv6 ~]# cat /etc/system-release
CentOS Linux release 7.4.1708 (Core)
[root@ccd-sdv6 ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

[root@ccd-sdv6 ~]# cat /etc/redhat-release
CentOS Linux release 7.4.1708 (Core)
[root@ccd-sdv6 ~]#
[root@ccd-sdv6 ~]# cat /etc/centos-release
CentOS Linux release 7.4.1708 (Core)
```

方法3： uname 命令<br>
uname(unix name的意思)是一个打印系统信息的工具，包括内核名称、版本号、系统详细信息以及所运行的操作系统等等。

	[root@ccd-sdv6 ~]# uname -a
	Linux ccd-sdv6 3.10.0_535872fd_c7d765c2_20180327 #2 SMP Tue Mar 27 15:41:30 EDT 2018 x86_64 x86_64 x86_64 GNU/Linux
	[root@ccd-sdv6 ~]#

方法4： /proc/version文件<br>
这个文件记录了Linux内核的版本、用于编译内核的gcc的版本、内核编译的时间，以及内核编译者的用户名。

	[root@ccd-sdv6 ~]# cat /proc/version
	Linux version 3.10.0_535872fd_c7d765c2_20180327 (root@x-kcloud1) (gcc version 4.9.4 (GCC) ) #2 SMP Tue Mar 27 15:41:30 EDT 2018
	[root@ccd-sdv6 ~]#

方法5：dmesg 命令<br>
dmesg (展示信息display message 或驱动程序信息driver message)是大多数类Unix操作系统上的一个命令，用于打印内核的消息缓冲区的信息。

```shell
[root@ccd-sdv6 ~]# dmesg |grep Linux
[    0.000000] Linux version 3.10.0_535872fd_c7d765c2_20180327 (root@x-kcloud1) (gcc version 4.9.4 (GCC) ) #2 SMP Tue Mar 27 15:41:30 EDT 2018
[    0.022548] SELinux:  Initializing.
[    0.026084] SELinux:  Starting in permissive mode
[    0.969100] [Firmware Bug]: ACPI: BIOS _OSI(Linux) query ignored
[    6.891810] SELinux:  Registering netfilter hooks
[    9.277581] Linux agpgart interface v0.103
[    9.458048] usb usb1: Manufacturer: Linux 3.10.0_535872fd_c7d765c2_20180327 xhci-hcd
[    9.576990] usb usb2: Manufacturer: Linux 3.10.0_535872fd_c7d765c2_20180327 xhci-hcd
[   12.684957] pps_core: LinuxPPS API ver. 1 registered
[   16.204674] SELinux:  Disabled at runtime.
[   16.209555] SELinux:  Unregistering netfilter hooks
[root@ccd-sdv6 ~]#
```


方法6-8：yum/dnf/rpm/apt-get都是用于软件包管理的工具

```shell
[root@ccd-sdv6 ~]# yum info tigervnc
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
Installed Packages
Name        : tigervnc
Arch        : x86_64
Version     : 1.8.0
Release     : 1.el7
Size        : 680 k
Repo        : installed
From repo   : c7-media
Summary     : A TigerVNC remote display system
URL         : http://www.tigervnc.com
License     : GPLv2+
Description : Virtual Network Computing (VNC) is a remote display system which
            : allows you to view a computing 'desktop' environment not only on the
            : machine where it is running, but from anywhere on the Internet and
            : from a wide variety of machine architectures.  This package contains a
            : client which will allow you to connect to other desktops running a VNC
            : server.

[root@ccd-sdv6 ~]#
```


​	