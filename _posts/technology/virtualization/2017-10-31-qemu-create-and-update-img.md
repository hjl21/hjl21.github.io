---
layout: post
title: qemu create and update img
date: 2017-10-31 09:20:23 +8000
categories: 技术文档
tag: Virtualization
---

\* content

{:toc}

# 1.qemu update

### KVM 

```shell
1.git clone git://vt-sync/qemu.git  ## clone qemu repo 
2.cd qemu.git
you can check which branch of qemu
git branch
you can checkout to uq/master tree
git checkout uq/master
3.then compile the qemu
./configure --target-list=x86_64-softmmu
./configure --target-list=x86_64-softmmu --enable-kvm  --enable-numa --enable-vnc --disable-gtk --disable-sdl # -display sdl(在这一步可以加上qemu的一些function比如--enable-numa[支持numa])
4.make -j 32(加j 10表示10个进程同时work) 
5.make install
```

### XEN 

```shell
./configure --enable-xen --target-list=i386-softmmu [for xen qemu的编译]
```

# 2.qemu创建img

```shell
1.qemu-img  create  -b 所依据的img名 -f  qcow2  自己所要的img名
example： 
qemu-img create -b /share/xvs/img/linux/ia32e_rhel7u2_kvm.img -f qcow2 ./rhel7u2_kvm_lmce.qcow2

2.做img步骤：
a.dd if=/dev/zero of=ia32e_rhel7u4_snapshort3_kvm.img bs=1M count=30000
或者 qemu-img  create  -f raw  /home/window8_2.img  60G

b.qemu-system-x86_64 -enable-kvm -cpu host -m 4028 -smp 4 -boot order=cd -hda /root/junliang/ia32e_rhel7u4_snapshort3_kvm.img -cdrom /root/junliang/RHEL-7.4-20170608.3-Server-x86_64-dvd1.iso
```
