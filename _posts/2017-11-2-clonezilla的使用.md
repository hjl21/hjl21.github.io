# CloneZilla的用法：

## 简介

Clonezilla,中文名再生龙，是一个用于系统备份和还原的工具。<br>
类似以Ghost，但它是开源、免费的。官网地址是http://www.clonezilla.org/

## 安装

1.下载一个clonezilla文件，有.iso和.zip两种格式，选择你所需要，download即可<br>
[clonezilla文件](http://www.clonezilla.org/downloads/download.php?branch=alternative)
http://www.clonezilla.org/downloads/download.php?branch=alternative<br>
2.可以通过UltraISO来安装到U盘,等一会就可以了。

## 备份

1.从U盘启动Clonezilla之后，首先进入的是如下界面：

![](http://img.blog.csdn.net/20170702121156535)
这里随便选择一个就可以了。<br>

2.之后进入的界面用于选择语言:
![](http://img.blog.csdn.net/20170702121403734)
之后根据自己的实际情况选择，之后的界面是以英语为基础的。

3.选择键盘键位，这个一般没有必要修改:默认
![](http://img.blog.csdn.net/20170702121655128)

4.模式选择，在这里可以选在图形界面还是命令行界面：<br>
对于初次使用者来说，还是使用图形界面比较好。 <br>
不过事实上最终图形界面选好后也会转成命令。

5.选择备份的双方，是将一个硬盘备份到另一个硬盘，还是将硬盘做成镜像文件：
![](http://img.blog.csdn.net/20170702132617419)
这里选择从硬盘备份到镜像。<br>

6.之后选择存放镜像的位置，这里需要做一个mount：
![](http://img.blog.csdn.net/20170702132846750)
这里选择本地设备，是为了选择U盘。

7.此时会出现一个界面来探测设备，可以插入新的U盘，等到设备被检测到了，就可以按Ctrl+C退出。
![](http://img.blog.csdn.net/20170702133048014)

8.下一个界面就可以选择设备，这里选择新插入的U盘：
![](http://img.blog.csdn.net/20170702133234334)

![](http://img.blog.csdn.net/20170702133530336)
选择好后就Done。

10.再之后就是选择需要备份的是硬盘本身还是里面的分区：
![](http://img.blog.csdn.net/20170702133754787)

11.之后，都是默认选择，结束后根据之前的选择（这里选择了reboot），就会提示你拔出U盘并确认。之后就会重启电脑：
![](http://img.blog.csdn.net/20170702134902859)

以上就是整个备份的过程。

## 还原

还原的过程跟备份差不多。

## 参考文献
[http://m.blog.csdn.net/jiangwei0512/article/details/73692007](http://m.blog.csdn.net/jiangwei0512/article/details/73692007)