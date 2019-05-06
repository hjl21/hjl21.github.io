---
layout: post
title: 重定向
date: 2017-11-15 15:24:23 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

# 重定向

### 1. linux shell下常用输入输出操作符是：

- 1.标准输入(STDIN,文件描述符为0)：默认从键盘输入，为0时表示是从其他文件或命令的输出。
- 2.标准输出(STDOUT,文件描述符为1)：默认输出到屏幕，为1时表示是文件。
- 3.错误输出(STDERR,文件描述符为2)：默认输出到屏幕，为2时表示是文件。

### 2. 输出重定向的情况：

	    符号                             作用
	
	命令 >  文件   将标准输出重定向到一个文件中(清空原有文件的数据)
	命令 2> 文件   将错误输出重定向到一个文件中(清空原有文件的数据)
	命令 >> 文件   将标准输出重定向到一个文件中(追加到原有内容的后面)
	命令 2>> 文件  将错误输出重定向到一个文件中(追加到原有内容的后面)
	
	命令 >> 文件 2>$1 将标准输出与错误输出共同写入到文件中(追加到原有内容的后面)

### 3. 输入重定向的情况：

	    符号                             作用
	
	命令 <  文件          将文件作为命令的标准输入
	命令 << 分界符        从标准输入中读入，直到遇到"分界符"才停止
	
	命令 << 文件1 >文件2  将文件1作为命令的标准输入并将标准输出到文件2

### 4. 例子

```shell
1)向aa文件写入一行文字：
[root@skl-s1 ~]# echo "junliang is a handsome boy" > aa
[root@skl-s1 ~]# vim aa
查看aa中的内容：
[root@skl-s1 ~]# cat aa
junliang is a handsome boy

2)把aa文件作为输入重定向给wc -l 命令来计算行数，命令等同于"cat aa |wc -l"
[root@skl-s1 ~]# wc -l < aa
1
[root@skl-s1 ~]#

3)向指定邮箱发送一封邮件，标题为welcome,内容逐行输入。
其中的over被称为分界符，是用户自定义的，当系统遇到这个分界符时会认为输入结束。

[root@skl-s1 ~]# mail -s "welcome" junliangx.huang@intel.com << over
> hello world
> welcome to intel
> over
[root@skl-s1 ~]#
正常情况下输入分界符后会结束输入操作并发送邮件，不会有报错信息。

向root用户发送一封邮件：
[root@skl-s1 ~]# echo "hello" | mail -s "welcome" root
查看邮件箱，果然有一封邮件：
[root@skl-s1 ~]# mail
Heirloom Mail version 12.5 7/5/10.  Type ? for help.
"/var/spool/mail/root": 1 message 1 new
>N  1 root                  Tue Nov 14 21:29  18/556   "welcome"

[备注]用echo、mail 和管道命令(|)发给root用户一封邮件，但内容只能有一句话。  
如果想像写信一样发邮件，就用输入重定向。
```
