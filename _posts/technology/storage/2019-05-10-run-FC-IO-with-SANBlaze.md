---
layout: post
title: Run FC IO with SANblaze
date: 2019-05-10 13:19:30 +0800
categories: 技术文档
tag: storage
---

* content
{:toc}

# Run FC IO with Sanblaze

### 1.create pool ###

#### a.create by GUI ####

![create pool]({{ '/styles/images/storage/create_pool.jpg' }})

### b.create by uemcli ####

~~~shell
uemcli -u admin -p <Your Password> -d <The array's Mgmt IP Address> /stor/config/pool create -name pool_test_1 -storProfile profile_23 -diskGroup dg_18 -drivesNumber 6
~~~

### 2.create lun

#### a.create by GUI ####

Block - LUNs: Click "create" buttom to create a LUN

![create lun]({{ '/styles/images/storage/create_lun.jpg' }})

#### b.create by uemcli ####

~~~shell
uemcli -u admin -p <Your Password> -d <The array's Mgmt IP Address> /stor/prov/luns/lun create -name lun_test_1 -pool pool_test_1 -size 10GB -thin yes
~~~

### 3.create host

#### a.create by GUI ####

![create host]({{ '/styles/images/storage/create_host.jpg' }})

#### b.create by uemcli ####

~~~shell
uemcli -u admin -p <Your Password> -d <The array's Mgmt IP Address> /remote/host create –name linux_host_1 –type host –addr 10.244.192.80 -osType Linux
~~~



### 4.setup host configuration

1.  添加已创建的lun

2. 添加initiators

   ![add initiators]({{ '/styles/images/storage/add_initiators.jpg' }})

3. initiator的个数及信息在SANBlaze上创建并查看

   ![initiator info]({{ '/styles/images/storage/initiator_info.jpg' }})

4. 如果网络没有问题，Initiator Paths会自动出现

**备注：SANBlaze可以模拟initiator和target,SFP用QLogic厂商的**