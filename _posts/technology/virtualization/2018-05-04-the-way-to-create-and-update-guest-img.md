---
layout: post
title: create and modify img 的方法
date: 2018-05-04 10:24:23 +8000
categories: 技术文档
tag: Virtualization
---

* content

{:toc}

## 1.制作guest image的方法

#### 1.1 dd方法 ####

1.dd if=/dev/zero of=ia32e_rhel7u4_snapshort3_kvm.img bs=1M count=30000 [做一个空的img,大小固定]
	2.qemu-system-x86_64 -enable-kvm -cpu host -m 4028 -smp 4 -boot order=cd \ 
	-hda /root/junliang/ia32e_rhel7u4_snapshort3_kvm.img -cdrom  \ 
	/root/junliang/RHEL-7.4-20170608.3-Server-x86_64-dvd1.iso

备注：运用dd命令也可以做u盘启动盘

```shell
bdw-ep1:~ # dd if=/root/ubuntu-17.04-server-amd64.iso of=/dev/sdb 
1402880+0 records in
1402880+0 records out
718274560 bytes (718 MB, 685 MiB) copied, 405.839 s, 1.8 MB/s
bdw-ep1:~ # eject /dev/sdb 

```

#### 1.2 qemu-img ####

1.qemu-img  create  -f raw  /home/window8_2.img   60G [做一个空的img，大小动态的]

2.qemu-system-x86_64 -enable-kvm -cpu host -m 4028 -smp 4 -boot order=cd \
	-hda /root/junliang/ia32e_rhel7u4_snapshort3_kvm.img -cdrom \
	/root/junliang/RHEL-7.4-20170608.3-Server-x86_64-dvd1.iso

[备注：按理说做一个空的img之后，最好格式化一下，如果不往里面装系统，用于挂载用的话，一定要格式化，否则会出错。]

3.qcow2文件

qemu-img create -b /share/xvs/img/ia32e_rhel7u3_kvm.img -f qcow2 test.qcow


## 2.如何快速修改img

在实际工作中，有时要对guest img进行修改，比如在guest img里面放一些工具，或者在rc.local文件里面加一些开机启动的命令，通常的做法是把guest img从server copy 到测试机上，然后在测试机上起guest,再把所需要的东西放到guest上，然后kill guest ,再把guest img 从测试机copy 到server上。这个过程相当的消耗时间。

下面介绍一种简单的方法：
直接在server上操作，也不用起guest。

```shell
[root@ccd-nfs images]# fdisk -l ia32e_rhel7u3_kvm.img

Disk ia32e_rhel7u3_kvm.img: 34.4 GB, 34359738368 bytes, 67108864 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0009d364

                Device Boot      Start         End      Blocks   Id  System
ia32e_rhel7u3_kvm.img1            2048     4202495     2100224   8e  Linux LVM
ia32e_rhel7u3_kvm.img2   *     4202496    67108863    31453184   83  Linux   [注意*号]
[root@ccd-nfs images]#
[root@ccd-nfs images]# ls test/
[root@ccd-nfs images]# mount -o loop,offset=2151677952 ia32e_rhel7u3_kvm.img ./test
[root@ccd-nfs images]# ls test/
1  bin  bm  boot  dev  etc  home  lib  lib64  Logs  lost+found  media  mnt  opt  proc  root  run  sbin  share  srv  sys  tmp  usr  var  xvs_export
[root@ccd-nfs images]#
注：先建个文件夹，直接把img挂载到该文件夹下面，然后在文件夹里面进行操作；
其中：2151677952=`4202496`*`512`

```
