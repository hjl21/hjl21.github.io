---
layout: post
title: yum install mysql
date: 2018-07-27 15:44:12 +8000
categories: 技术文档
tag: mysql
---

* content
{:toc}

## 如何安装mysql在linux OS上
1.下载rpm包：
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz<br>

2.解压：tar zxvf mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz<br>

3.复制解压后的mysql目录：cp -r mysql-5.6.33-linux-glibc2.5-x86_64 /usr/local/mysql<br>

4.安装：<br>

	cd /usr/local/mysql/
	mkdir ./data/mysql
	chown -R mysql:mysql ./
	 ./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data/mysql 初始化数据库
	cp support-files/mysql.server /etc/init.d/mysqld
	chmod 755 /etc/init.d/mysqld
	cp support-files/my-default.cnf /etc/my.cnf


5.修改启动脚本

vi /etc/init.d/mysqld

修改项：<br>

basedir=/usr/local/mysql/
datadir=/usr/local/mysql/data/mysql

	[root@junliang mysql]# cp support-files/mysql.server /etc/init.d/mysqld
	[root@junliang mysql]# chmod 755 /etc/init.d/mysqld
	[root@junliang mysql]# cp support-files/my-default.cnf /etc/my.cnf
	cp: overwrite ‘/etc/my.cnf’? yes
	[root@junliang mysql]# vim /etc/init.d/mysqld
	[root@junliang mysql]# service mysqld start
	Unit mysqld.service could not be found.
	Starting MySQL. SUCCESS!

6.常见问题解决：

1.[root@junliang mysql]# mysql -uroot
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

解决方法：<br>
一、在my.cnf 中添加指定 mysql.sock文件

```shell
[root@junliang mysql]# vim /etc/my.cnf
[mysqld]
socket=/tmp/mysql.sock
```
如果还是不行，用第二种方法<br>
二、添加软连接<br>

```shell
[root@junliang mysql]# mkdir -pv /var/lib/mysql
[root@junliang mysql]# ln -s /tmp/mysql.sock /var/lib/mysql/mysql.sock
```

2.[root@junliang mysql]# mysql
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

解决方法：

```shell
[root@junliang mysql]# mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
[1] 16079
[root@junliang mysql]# 180726 22:42:25 mysqld_safe Logging to '/var/lib/mysql/junliang.err'.
180726 22:42:25 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
[root@junliang mysql]#
```

7.注意事项：<br>
1.添加用户组和用户(解压完rpm包后，一般会自动创建Mysql用户组)

```shell
#添加用户组

groupadd mysql

#添加用户mysql 到用户组mysql

useradd -g mysql mysql 
```

2.设置root用户密码

```shell
./bin/mysqladmin -u root password '123456'
```
