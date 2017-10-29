#VIM使用
###1.替换字符串
	：.，$ s/str1/str2/g 用字符串 str2 替换正文当前行到末尾所有出现的字符串 str1 
	 
	：1，$ s/str1/str2/g 用字符串 str2 替换正文中所有出现的字符串 str1 

###2.导入文件
	如要将a.txt的内容拷贝到b.txt中，可以执行如下步骤：
	用vim打开b.txt。将光标定位到要复制插入的位置，然后进入命令模式中输入
	:r!cat a.txt
	保存退出：wq！搞定～～
###3.删除当前行到结尾的全部内容
	在命令模式下，输入:.,$d 一回车就全没了。
	
	表示从当前行到末行全部删除掉
###4.Taglist
	Taglist 安装:http://www.cnblogs.com/caosiyang/archive/2011/12/23/2299190.html
	Taglist源码下载:https://vim.sourceforge.io/scripts/script.php?script_id=273
	Taglist使用：http://blog.csdn.net/daniel_ustc/article/details/8299096
###5.vimrc(/root/.vimrc)
	set nu
	syntax on
	set ai   [自动对齐功能]
	set sw=4 [shiftwidth缩进量长度为4]
	set ts=4 [tabstop制表符的长度为4]
