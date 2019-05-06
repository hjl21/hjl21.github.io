---
layout: post
title: SLES12整理
date: 2017-10-31 14:24:23 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}



# SLES12整理

## A.装完系统后的准备工作

```shell
1.新安装的os,配置yast2源用SLE-12-SP2-SDK-DVD-x86_64-GM-DVD1.iso[python-devel]
SLE-12-SP2-Server-DVD-x86_64-GM-DVD1.iso[rpm-build]两个iso文件;如果iso配不上去，
可以用mount试试
	mount SLE-12-SP1-SDK-DVD-x86_64-Beta2-DVD1.iso /test
	cd /test/suse/x86_64
2.新安装的os,/etc/xen/目录下面没有xlexample.*3个文件(会导致起guest fail没有mac)，
可以从其他机器copy 一份过来;
3.安装rpm包,用zypper类似redhat的yum;
	a. zypper install nmap [因为用nc连接switch]
	b. zypper install python-devel[for XVS install]
	c. zypper install rpm-build[for XVS install]
	d. zypper install xen-devel [for 编译residency]acpi P_status and C_status cases

4.配置网桥
BOOTPROTO='dhcp' 把多余的网桥删掉;
5.Sles12添加开机自启动的内容与Redhat不同，不用创建rc.local文件(系统也没有这个文件)，
在/etc/rc.d/boot.local文件中加上所需的开机自启动内容；
6.关闭防火墙
	SuSEfirewall2 stop

7.时间，如果时间不对会影响一些fail
	date -s 2017-4-12
	date -s 10:35:00
8./usr/lib/rpm/macros 
以上几步是对刚装os后要做的操作，完成后就可以安装XVS and run cases.
9.对于control_panel XEN_1024M_pvgrub case,要从nightly的机器上copy /boot/pv-grub-x86_64.gz这个文件
```

## B.UEFI and Legacy model

```shell
KVM 装完系统后用默认的grub启动项就可以使用；
XEN 装完系统后需要在选择Xen hypervisor这个启动项，然后在里面加上domin 0的信息；  
UEFI模式同redhat类似，是在/boot/efi/EFI/sles/目录下对应cfg文件里面加上domin 0的信息
例如：
	config.1]
  	options=dom0_mem=4096M dom0_max_vcpus=4 loglvl=all guest_loglvl=all unrestricted_guest=1 msi=1 conring_size=4M  console=com1 com1=115200,8n1 sync_console psr=cmt psr=cat psr=cdp ept=pml cpufreq=performance vpid=1 vpmu=1 altp2m=1
   	kernel=vmlinuz-4.4.21-69-default root=UUID=6030b3e0-6f12-428a-a1ce-b5948002aba8  ro crashkernel=auto console=com1 com1=115200,8n1 intel_iommu=on
   ramdisk=initrd-4.4.21-69-default
	[config.2]

这一点特别像redhat UEFI模式 xen.cfg文件
```

