# serial log

## 1.host log

在grub.cfg中加上console=tty0 console=ttyS0,115200n8，就可以拿到机器的串口log

如：BOOT_IMAGE=/vmlinuz-3.10.0 root=/dev/mapper/rhel-root ro pcm=0x800000000@0x880000000 pcm_sample_window=30000 crashkernel=auto rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap intel_iommu=on LANG=en_US.UTF-8 ```console=tty0 console=ttyS0,115200n8``` pcmtest=mtr-replay

## 2.guest log
### 2.1 kvm guest
 
2.1.1 把guest grub.cfg文件中"rhgb quiet" to "console=ttyS0,115200,8n1"<br>
2.1.2 起guest: <br>
qemu-system-x86_64 -enable-kvm -m 4096 -smp 4 -sgx epc=512K -serial pty 
-cpu host -device virtio-net-pci,netdev=nic0,mac=00:16:3e:0c:12:78 
-netdev tap,id=nic0,script=/etc/kvm/qemu-ifup 
-drive file=ubuntu_sgx.qcow2,if=none,id=virtio-disk0 
-device virtio-blk-pci,drive=virtio-disk0<br>

	#-serial stdio直接输出到屏幕上
2.1.3连接串口

minicom -D /dev/pts/X

### 2.2 xen guest

2.2.1 把guest grub.cfg文件中"rhgb quiet" to "console=ttyS0,115200,8n1"<br>
2.2.2 起guest:(```xl create -c config.cfg```) <br>

	[root@l1xen haibin]# cat config.test
	builder = "hvm"
	name = "lmce"
	vcpus = 2
	memory = 1024
	disk = [ '/dev/sdb, raw, xvda, rw' ]
	cpus = [ '2', '3' ]
	device_model_override = '/usr/local/lib/xen/bin/qemu-system-i386'
	device_model_version = 'qemu-xen'
	sdl = 0
	vnc = 1
	vnclisten = '0.0.0.0'
	stdvga = 1
	hap = 1
	hpet = 1
	serial = 'pty'
	on_crash = 'preserve'
	lmce = 1
2.2.3 连接串口

xl console domid
