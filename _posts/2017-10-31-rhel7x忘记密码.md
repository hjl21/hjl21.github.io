# rhel7.X 忘记密码

### init方法：

	1. 启动系统，并在GRUB2启动屏显时，按下e键进入编辑模式。
	2. 在linux16/linux/linuxefi所在参数行ro更改为init=/sysroot/bin/sh
	3. 按Ctrl+x启动到shell。
	4. 挂载文件系统为可写模式：mount –o remount,rw /sysroot
	5、换根 chroot /sysroot
	6. 运行passwd,并按提示修改root密码。
	7.如何之前系统启用了selinux，必须运行以下命令，否则将无法正常启动系统：touch /.autorelabel
	8、exit退出
	9、reboot 重启

### rd.break方法

	1、启动的时候，在启动界面，相应启动项，内核名称上按“e”；
	
	2、进入后，找到linux16开头的地方，按“end”键到最后，输入rd.break，按ctrl+x进入；
	
	3、进去后输入命令mount，发现根为/sysroot/，并且不能写，只有ro=readonly权限；
	
	4、mount -o remount,rw /sysroot/，重新挂载，之后mount，发现有了r,w权限；
	
	5、chroot /sysroot/ 改变根；
	
	（1）echo RedHat|passwd –stdin root 修改root密码为redhat，或者输入passwd，交互修改；
	
	（2）还有就是先cp一份，然后修改/etc/shadow文件
	
	6、touch /.autorelabel 这句是为了selinux生效
	
	7、ctrl+d 退出
	
	8、然后reboot
	
	至此，密码修改完成
