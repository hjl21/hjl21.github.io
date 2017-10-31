### KVM INstall

###a.Compile KVM

	git clone git://vt-sync/kvm.git
	cd kvm   
	copy config to linux
	例如：cp config_for_kvm .config

### b.Complie new kernel and install

	1.make menuconfig
	2.make -j 32 
	3.make modules_install && make install
	4.grub2-mkconfig -o /boot/grub2/grub.cfg[不用做，只是为了添加启动项]

# XEN Install

### a.Compile
 
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

### b.Excute configure 

	[root@vt xen]#./configure ###default 

### Make Xen

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

#Build dom0

### Download linux.git

	[root@vt boot]#cd /home/build/ 
	[root@vt build]#git clone git://vt-sync.sh.intel.com/linux-stable.git  
	  Down latest kernel config file, and rename it to /home/build/linux/.config config-3.9.3--its a example configure file

### Make Linux kernel

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
#after xen and dom0,system configuration info

### 1.启动xen
	[root@vt /]#echo "/usr/local/lib" >>/etc/ld.so.conf
	[root@vt /]#ldconfig
	[root@vt /]# /etc/init.d/xencommons start
	#we also can put 3 command to /etc/rc.d/rc.local, then give rc.local execute(chmod +x /etc/rc.d/rc.local)

###2.create vm
	1.xl list
	2.xl info 
	3.xl create + 配置信息
	4.xl vnc + domainID

###一些问题解决办法
	1.查看网桥信息：brctl show，如果有两个，把/etc/libvirt/qemu/networks 下的default.xml换个名字 或者把virbr0删掉
	2.查看host的cpu信息：xenpm get-cpu-topolo
	3.查看host的mem信息: xl info |grep mem
	4.xl list执行不了
		rm -rf /var/run/xen*
		echo "/usr/local/lib" >>/etc/ld.so.conf
		ldconfig
		/etc/init.d/xencommons start
	5.判断KVM or XEN 环境的依据：/usr/libexec/里面qemu-kvm
