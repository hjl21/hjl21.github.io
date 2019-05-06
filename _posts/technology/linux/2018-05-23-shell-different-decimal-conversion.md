---
layout: post
title: shell各进制之间转换
date: 2018-05-23 13:24:00 +8000
categories: 技术文档
tag: Linux
---



* content

{:toc}

### shell各进制之间转换 ###

#### 16进制转换10进制 ####

```shell
start_phys_addr=`echo $((16#$start_phys_addr))`
end_phys_addr=`echo $((16#$end_phys_addr))`
```

#### 10进制转换16 ####

```shell
start_phys_addr=`echo "obase=16;$start_phys_addr" | bc`
end_phys_addr=`echo "obase=16;$end_phys_addr" |bc`
```

#### 16进制进行计算 ####

```shell
start_phys_addr=`echo "obase=16;ibase=16;$start_phys_addr+40" |bc`
```

### 参考文献 ###

[https://phoxis.org/2012/07/12/builtin-bash-any-base-to-decimal-conversion/]()