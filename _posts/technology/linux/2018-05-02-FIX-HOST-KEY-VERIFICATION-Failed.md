---
layout: post
title: FIX HOST KEY VERIFICATION Failed
date: 2018-05-02 11:24:23 +8000
categories: 技术文档
tag: Linux
---



# SSH中"HOST KEY VERIFICATION FAILED."的解决方案

我们使用ssh链接linux主机时，可能出现"Host key verification failed."的提示，ssh连接不成功。
可能的提示信息如下:


```shell
@ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
23:00:20:83:de:02:95:f1:e3:34:be:57:3f:aa:aa:aa.
Please contact your system administrator.
Add correct host key in /home/haha/.ssh/known_hosts to get rid of this message.
Offending key in /home/haha/.ssh/known_hosts:8
RSA host key for localhost has changed and you have requested strict checking.
Host key verification failed..
```


网上很多的解决方案是：vi ~/.ssh/known_hosts 删除与想要连接的主机相关的行；或者直接删除known_hosts这个文件。<br>
当然这个方案也是可行的，但并非解决问题的根本办法，因为继续使用，今后还可能会出现这样的情况，还得再删除。

下面简单讲一下这个问题的原理和比较长久的解决方案。

用OpenSSH的人都知ssh会把你每个你访问过计算机的公钥(public key)都记录在~/.ssh/known_hosts。<br>

当下次访问相同计算机时，OpenSSH会核对公钥。如果公钥不同，OpenSSH会发出警告，避免你受到DNS Hijack之类的攻击。<br>SSH对主机的public_key的检查等级是根据StrictHostKeyChecking变量来配置的。默认情况下，StrictHostKeyChecking=ask。<br>简单说下它的三种配置值：

1.StrictHostKeyChecking=no  #最不安全的级别，当然也没有那么多烦人的提示了，相对安全的内网测试时建议使用。<br>如果连接server的key在本地不存在，那么就自动添加到文件中（默认是known_hosts），并且给出一个警告。

2.StrictHostKeyChecking=ask #默认的级别，就是出现刚才的提示了。如果连接和key不匹配，给出提示，并拒绝登录。

3.StrictHostKeyChecking=yes #最安全的级别，如果连接与key不匹配，就拒绝连接，不会提示详细信息。

对于我来说，在内网的进行的一些测试，为了方便，选择最低的安全级别。在.ssh/config（或者/etc/ssh/ssh_config）中配置：

```shell
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```

（注：这里为了简便，将knownhostfile设为/dev/null，就不保存在known_hosts中了）

