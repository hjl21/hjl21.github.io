---
layout: post
title: KVM&Xen Install
date: 2018-07-31 10:20:23 +8000
categories: 技术文档
tag: Virtualization
---


* content

{:toc}


# 1.KVM Install

### 1.1 Compile KVM

```shell
git clone git://vt-sync/kvm.git
cd kvm   
copy config to linux
例如：cp config_for_kvm .config
```

### 1.2 Complie new kernel and install

```shell
1.make menuconfig
2.make -j 32 
3.make modules_install && make install
4.grub2-mkconfig -o /boot/grub2/grub.cfg[不用做，只是为了添加启动项]
```

# 2.XEN Install

### 2.1 Compile

```shell
git clone git://vt-sync.sh.intel.com/xen.git
cd xen

**modify the config disk type: xvda—>hda**

sed -i '/disk/s/xvda/hda/g' tools/examples/xlexample.hvm
sed -i '/disk/s/xvda/hda/g' tools/examples/xlexample.pvlinux

**Update the context inside**

sed -i 's$OVMF_UPSTREAM_URL ?=.*$OVMF_UPSTREAM_URL=git://vt-sync/ovmf.git$' ./Config.mk
sed -i 's$QEMU_TRADITIONAL_URL ?=.*$QEMU_TRADITIONAL_URL=git://vt-sync/qemu-xen-traditional.git$' ./Config.mk
sed -i 's$QEMU_UPSTREAM_URL ?=.*$QEMU_UPSTREAM_URL ?= git://vt-sync/qemu-xen.git$' ./Config.mk
sed -i 's$SEABIOS_UPSTREAM_URL ?=.*$SEABIOS_UPSTREAM_URL ?= git://vt-sync/seabios.git$' ./Config.mk
sed -i 's$MINIOS_UPSTREAM_URL ?=.*$MINIOS_UPSTREAM_URL ?= git://vt-sync/mini-os.git$' ./Config.mk
sed -i 's/3403ac4313812752be6e6aac35239ca6888a8cab/2e11d582b5e14759b3c1482d7e317b4a7257e77d/' ./Config.mk

**Export http_proxy**

export http_proxy="http://proxy-shz.intel.com:911"
export GIT_HTTP="y"
```

### 2.2 Excute configure 

```shell
[root@vt xen]#./configure ###default 
```

### 2.3 Make Xen

```shell
[root@vt xen]#make xen -j $num_cpu  # '-j $num_cpu'
  Make tools, install xen & tools  
[root@vt xen-unstable.hg]#make tools -j $num_cpu  
[root@vt xen-unstable.hg]#make install-xen  
[root@vt xen-unstable.hg]#make install-tools 
  Verify. After finishing installing xen & tools, new files are generated in /boot directory
[root@vt xen-unstable.hg]#cd /boot 
[root@vt-ivt1 boot]#ll -tr 

-rw-r--r--. 1 root root 13313173 May 24 21:19 xen-syms-4.6-unstable 
-rw-r--r--. 1 root root   793467 May 24 21:19 xen-4.6-unstable.gz 
lrwxrwxrwx. 1 root root       19 May 24 21:19 xen-4.6.gz -> xen-4.6-unstable.gz 
lrwxrwxrwx. 1 root root       19 May 24 21:19 xen.gz -> xen-4.6-unstable.gz 
```

# 3 Build dom0

### 3.1 Download linux.git

```shell
[root@vt boot]#cd /home/build/ 
[root@vt build]#git clone git://vt-sync.sh.intel.com/linux-stable.git  
  Down latest kernel config file, and rename it to /home/build/linux/.config config-3.9.3--its a example configure file
```

### 3.2 Make Linux kernel

```shell
[root@vt build]#scp xen-build.sh.intel.com:/home/build/repo/config-example linux-stable/.config 
[root@vt build]#cd linux-stable 
[root@vt linux-stable]#echo "" | make oldconfig  
[root@vt linux-stable]#make -j 32 
[root@vt linux-stable]#make modules_install  
[root@vt linux-stable]#make install  
. Verify. After finishing installing dom0, new files are generated in /boot directory
[root@vt linux]cd /boot 
[root@vt boot]ll -tr 
	-rw-r--r--. 1 root root  5084960 May 24 22:26 vmlinuz-4.1.1
	-rw-r--r--. 1 root root  2483412 May 24 22:26 System.map-4.1.1 
	lrwxrwxrwx. 1 root root       20 May 24 22:26 vmlinuz -> /boot/vmlinuz-4.1.1
	lrwxrwxrwx. 1 root root       23 May 24 22:26 System.map -> /boot/System.map-4.1.1 
	-rw-r--r--. 1 root root  4863943 May 24 22:26 initramfs-4.1.1.img 
最后修改grub.cfg文件，reboot
```
## 4.after xen and dom0,system configuration info

### 4.1 启动xen
	[root@vt /]#echo "/usr/local/lib" >>/etc/ld.so.conf
	[root@vt /]#ldconfig
	[root@vt /]# /etc/init.d/xencommons start
	#we also can put 3 command to /etc/rc.d/rc.local, then give rc.local execute(chmod +x /etc/rc.d/rc.local)

### 4.2 create vm

```shell
1.xl list
2.xl info 
3.xl create + 配置信息
4.xl vnc + domainID
```

### 4.3 一些问题解决办法

```shell
1.查看网桥信息：brctl show，如果有两个，把/etc/libvirt/qemu/networks 下的default.xml换个名字 或者把virbr0删掉
2.查看host的cpu信息：xenpm get-cpu-topolo
3.查看host的mem信息: xl info |grep mem
4.xl list执行不了
	rm -rf /var/run/xen*
	echo "/usr/local/lib" >>/etc/ld.so.conf
	ldconfig
	/etc/init.d/xencommons start
5.判断KVM or XEN 环境的依据：/usr/libexec/里面qemu-kvm
```

# 5.安装过程中出现的error及解决办法：

- 5.1 yum install xz-devel for <font color=red size=9>lzma</font> 缺少	
- 5.2 yum install uuid-devel.x86_64 for <font color=red size=9>uuid</font> 缺少
- 5.3 yum install ncurses-devel.x86_64 for <font color=red size=5>ncurses</font> 缺少
- 5.4 yum install glib2-devel.x86_64  for <font color=red size=5>glib 2.0</font>     缺少
- 5.5 yum install pixman-devel.x86_64 for <font color=red size=5>pixman</font> 缺少
- 5.6 yum install glibc-devel.i686 for <font color=red size=5>stubs-32.h文件缺失</font>
- 5.7 如果出现这样的error:  
	Error:  Multilib version problems found.
	Protected multilib versions: nss-softokn-freebl-3.16.2.3-13.el7_1.i686 != nss-softokn-freebl-3.16.2.3-14.el7.x86_64
	<font color=red size=5>解决办法</font>：1.yum distribution-synchronization 2.yum install glibc-devel.i686
- 5.8 yum install openssl-devel for <font color=red size=5>fatal error: openssl/opensslv.h: No such file or directory</font>
- 5.9 缺少uuid.h的解决办法：
	error: uuid/uuid.h: No such file or directory
	 	<font color=red size=5>解决办法:</font>
	1).yum install uuid uuid-devel  
	2).yum install e2fsprogs-devel  
	3).yum install libuuid libuuid-devel



