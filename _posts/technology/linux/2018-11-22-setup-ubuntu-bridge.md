---
layout: post
title: Ubuntu网桥设置
date: 2018-11-22 16:00:23 +8000
categories: 技术文档
tag: Linux
---

* content
{:toc}

# Ubuntu网桥设置

## 允许root登录：

```shell
Ubuntu允许root登录：

vi /etc/ssh/sshd_config

PermitRootLogin yes （默认为#PermitRootLogin prohibit-password）
：wq

service ssh restart
```

since Netplan replaced ifupdown as the default configuration utility.<br>
starting with Ubuntu 17.10, we use netplan to configure the bridge.

For more details: https://netplan.io/

## ubuntu 17.10设置网桥

```shell
1.cd /etc/netplan
2.vim 01-netcfg.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s31f6:
      dhcp4: no

  bridges:
    virbr0:
      dhcp4: yes
      parameters:
        stp: true
      interfaces: [enp0s31f6]

3.netplan apply
```

## ubuntu 17.10之前的网桥配置

```shell
1.vim /etc/network/interfaces 

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
  iface lo inet loopback

# The primary network interface
auto enp59s0f0
  iface enp59s0f0 inet dhcp

auto enp0s31f6
  iface enp0s31f6 inet manual

auto virbr0
  iface virbr0 inet dhcp
  bridge_ports enp0s31f6
  bridge_stp on
  bridge_fd 0
  bridge_maxwait 0

2./etc/init.d/networking restart
```

