---
layout: post
title: 用ssh实现免登陆Linux
date: 2018-05-02 14:24:00 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

### 用ssh实现免登陆Linux ###

#### 1.ssh是什么? ####

SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用SSH 协议可以有效防止远程管理过程中的信息泄露问题。

#### 2.准备工作 ####

假设A主机192.168.199.123想通过ssh登录到B主机192.168.199.209上。<br>
那么客户端（A主机）需要安装ssh客户端软件，服务器端（B主机开机sshd进程）需要安装ssh服务器软件。<br>
ssh客户端Linux发行版一般都自带的，对于ssh服务器端，Ubuntu用户可以sudo apt-get install openssh-server来安装，<br>其他Linux用户也安装openssh-server即可。
在各自的$HOME目录下都有一个.ssh隐藏目录。

#### 3.生成密钥 ####

在A主机，cd $HOME/.ssh
`运行命令ssh-keygen -t dsa 生成密钥。（dsa换成rsa，即是使用rsa加密算法生成密钥了）
 一路Enter键确认下来，当然遇到"yes/no?"一般选yes就好了。
`

~~~shell
[root@cicada-serial-port ~]# cd .ssh/

[root@cicada-serial-port .ssh]# ls

id_dsa  `id_dsa.pub`  known_hosts
~~~

#### 4.分发密钥（公钥) ####

在A主机，scp ~/.ssh/id_dsa.pub  userB@192.168.199.209:~/.ssh/id_dsa.pub
此过程中需要输入B主机的userB的密码。<br>
然后，在B主机：cat ~/.ssh/id_dsa.pub  >> ~/.ssh/authorized_keys (将id_dsa.pub的内容追加到 authorized_keys中)

#### 6.享受免登陆 ####

在A主机：ssh userB@192.168.199.209  不需输入密码即可登录B主机；<br>
当然 scp test userB@192.168.199.209 远程拷贝也不需输入密码了（scp基于ssh协议嘛）；<br>
还可以ssh远程执行B主机上的命令或执行脚本，比如：<br>
ssh test userB@192.168.199.209 '/etc/init.d/network restart' #重新启动B主机上的网络服务；<br>
远程执行多条命令可用分号隔开，如：ssh test userB@192.168.199.209 'ls;pwd'


```shell
	[root@cicada-serial-port .ssh]# ssh 192.168.199.209
	Warning: Permanently added '192.168.199.209' (ECDSA) to the list of known hosts.
	Last login: Thu Jul 20 01:07:06 2017 from 10.239.48.248
	ABRT has detected 2 problem(s). For more info run: abrt-cli list --since 1500527226
	[root@ccd-sdv4 ~]#
```
