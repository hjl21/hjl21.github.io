---
layout: post
title: shell中变量自增的实现方法
date: 2018-05-29 15:14:33 +8000
categories: 技术文档
tag: Linux
---



* content

{:toc}

## shell中变量自增的实现方法：

```shell
1. i=`expr $i + 1`;
2. let i+=1;
3. ((i++));
4. i=$[$i+1];
5. i=$(( $i + 1 ))
```


```shell
#!/bin/bash
i=0
while [ $i -lt 4 ]
do
   echo $i
   i=`expr $i + 1`
   # let i+=1
   # ((i++))
   # i=$[$i+1]
   # i=$(( $i + 1 ))
done
```

另外，对于固定次数的循环，可以通过seq命令来实现，就不需要变量的自增了

```shell
#!/bin/bash
for j in $(seq 1 5)
do
  echo $j
done
```