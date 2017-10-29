#shell常用命令
	1.find ./ -name  '*' | grep "qemu" 
	2.linux 删除不同文件夹内相同的文件
		find A -name c.txt | xargs rm -rf
		#之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了xargs命令
	3.2>&1 > /dev/null
		#不让信息显示在屏幕，不论正确与否，都输出到/dev/null

 