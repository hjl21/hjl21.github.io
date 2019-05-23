---
layout: post
title: linux screen tool
date: 2019-05-23 15:39:40 +0800
categories: 技术文档
tag: Linux
---

* content
{:toc}
## linux screen tool ##

### 1.背景 ###

系统管理员经常需要SSH 或者telent 远程登录到Linux 服务器，经常运行一些需要很长时间才能完成的任务，比如系统备份、ftp 传输等等。通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为它们执行的时间太长了。必须等待它们执行完毕，在此期间不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切就半途而废，在实际情况中经常遇到窗口断掉的情况。

### 2.简介 ###

**GNU Screen**是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。

### 3.安装screen ###

~~~shell
yum install screen -y
~~~

### 4.常用参数及命令 ###

~~~shell
-A 　将所有的视窗都调整为目前终端机的大小。
-d   <作业名称> 　将指定的screen作业离线。
-h   <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r   <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S   <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。

screen -S yourname ->    新建一个叫yourname的session
screen -ls ->            列出当前所有的session
sles12-huangj43-dev-00:/home/test/log # screen -ls
There is a screen on:
        10964.top       (Detached)
1 Socket in /var/run/screens/S-root.

sles12-huangj43-dev-00:/home/test/log #
screen -r yourname ->    重新连接yourname这个session
sles12-huangj43-dev-00:/home/test/log # screen -r top

screen -d yourname ->    远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
~~~

### 5.screen session 操作 ###

**在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始。**

1. C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里，每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。

2. C-a k -> kill window，强行关闭当前的 window

3. C-a c -> 创建一个新的运行shell的窗口并切换到该窗口

4. C-a n -> Next，切换到下一个 window 

5. C-a p -> Previous，切换到前一个 window 

6. C-a [ -> 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样

   ​    C-b Backward，PageUp 

   ​    C-f Forward，PageDown 

   ​    H(大写) High，将光标移至左上角 

   ​    L Low，将光标移至左下角

   ​    0 移到行首 

   ​    $ 行末 

   ​    w forward one word，以字为单位往前移 

   ​    b backward one word，以字为单位往后移 

   ​    Space 第一次按为标记区起点，第二次按为终点 

   ​    Esc 结束 copy mode 

   C-a ] -> Paste，把刚刚在 copy mode 选定的内容贴上

### 6.清除dead会话或者杀掉某个会话 ###

~~~shell
sles12-huangj43-dev-00:/home/test/log # screen -ls
There are screens on:
        12217.top       (Detached)
        10964.top       (Detached)
2 Sockets in /var/run/screens/S-root.

sles12-huangj43-dev-00:/home/test/log # kill -9 12217
sles12-huangj43-dev-00:/home/test/log # screen -ls
There are screens on:
        12217.top       (Dead ???)
        10964.top       (Detached)
Remove dead screens with 'screen -wipe'.
2 Sockets in /var/run/screens/S-root.

sles12-huangj43-dev-00:/home/test/log # screen -wipe
There are screens on:
        12217.top       (Removed)
        10964.top       (Detached)
1 socket wiped out.
1 Socket in /var/run/screens/S-root.

sles12-huangj43-dev-00:/home/test/log # screen -ls
There is a screen on:
        10964.top       (Detached)
1 Socket in /var/run/screens/S-root.

sles12-huangj43-dev-00:/home/test/log # screen -r 10964
~~~

### 7.分屏 ###

将一个屏幕分割成不同区域显示不同的Screen窗口显然是个很酷的事情。可以使用快捷键C-a S将显示器水平分割，Screen 4.00.03版本以后，也支持垂直分屏，快捷键是C-a |。分屏以后，可以使用C-a \<tab\>在各个区块间切换，每一区块上都可以创建窗口并在其中运行进程。

可以用C-a X快捷键关闭当前焦点所在的屏幕区块，也可以用C-a Q关闭除当前区块之外其他的所有区块。关闭的区块中的窗口并不会关闭，还可以通过窗口切换找到它。

例如：

按**CTRL+a，tab**可以在两个屏幕之间自由切换：

![切换]({{ '/styles/images/screen1.jpg' }})

切换到下个屏幕后，没有命令输入的提示符啊，怎么建立呢？

按**CTRL+a c**

![添加输入提示符]({{ '/styles/images/screen2.jpg' }})

**备注**

分屏常用的操作：
>
>Ctrl a c       创建新窗口
>
> Ctrl a n       切换到下个窗口 
>
>Ctrl a p       上个窗口
>
> Ctrl a “       显示窗口列表

### 8.C/P模式和操作 ###

screen的另一个很强大的功能就是可以在不同窗口之间进行复制粘贴。使用快捷键C-a \<Esc\>或者C-a [可以进入copy/paste模式，这个模式下可以像在vi中一样移动光标，并可以使用空格键设置标记。其实在这个模式下有很多类似vi的操作，譬如使用/进行搜索，使用y快速标记一行，使用w快速标记一个单词等。

一般情况下，可以移动光标到指定位置，按下空格设置一个开头标记，然后移动光标到结尾位置，按下空格设置第二个标记，同时会将两个标记之间的部分储存在copy/paste buffer中，并退出copy/paste模式。在正常模式下，可以使用快捷键C-a ]将储存在buffer中的内容粘贴到当前窗口。