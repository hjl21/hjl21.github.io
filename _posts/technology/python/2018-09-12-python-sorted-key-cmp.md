---
layout: post
title: python sort sorted key cmp详解
date: 2018-09-12 11:54:00 +8000
categories: 技术文档
tag: Python
---

* content
{:toc}

# python sort sorted key cmp详解

sort() 是list的内置方法，只有list有<br> 
sorted()方法是Python内置的，可以对所有可迭代的序列排序生成新的序列，只要可迭代就行，返回的都是一个list

## sorted

sorted(itrearble, cmp=None, key=None, reverse=False)<br> 
默认的是升序排列，如果要降序排列，直接sorted(ls,reverse=True)
	

```python
sorted('123456')  字符串

['1', '2', '3', '4', '5', '6']

sorted([1,4,5,2,3,6])  列表
[1, 2, 3, 4, 5, 6]

sorted({1:'q',3:'c',2:'g'}) 字典， 默认对字典的键进行排序
[1, 2, 3]

 sorted({1:'q',3:'c',2:'g'}.keys())  对字典的键
[1, 2, 3]

sorted({1:'q',3:'c',2:'g'}.values())  对字典的值
['c', 'g', 'q']

sorted({1:'q',3:'c',2:'g'}.items())  对键值对组成的元组的列表
[(1, 'q'), (2, 'g'), (3, 'c')]
```


## key的使用
主要是用于指定需要比较的内容，但是如果不指定cmp的话，是使用默认的升序排序，也就是对于选定的key进行升序排列

```python
>>>a = [('a', 1), ('b', 2), ('c', 6), ('d', 4), ('e', 3)]
>>> a
[('a', 1), ('b', 2), ('c', 6), ('d', 4), ('e', 3)]
>>>sorted(a, key=lambda x:x[0])
[('a', 1), ('b', 2), ('c', 6), ('d', 4), ('e', 3)]
```

这里，列表里面的每一个元素都为二维元组，key参数传入了一个lambda函数表达式，其x就代表列表里的每一个元素，然后分别利用索引返回元素内的第一个和第二个元素，这就代表了sorted()函数利用哪一个元素进行排列.如果不指定cmp的话，是使用默认的升序排序，也就是对于选定的key进行升序排列.


## cmp的使用

cmp指定一个定制的比较函数，这个函数接收两个参数（iterable的元素），如果第一个参数小于第二个参数，返回一个负数；如果第一个参数等于第二个参数，返回零；如果第一个参数大于第二个参数，返回一个正数。默认值为None。<br>

 可以这么理解，cmp()参数中第一个值比第二个值小，如果是b-a这种形式，也是第一个比第二个小，那么其实就是升序； 
 除了自己指定cmp函数外， 还有一种简便的写法:<br>

	sorted(list,cmp=lambda x,y:cmp(y,x))


借助原先的cmp函数进行一个简单的逆序（从大到小）

```python
>>> sorted(list1,cmp = lambda x,y : cmp(y[0],x[0]))
Traceback (most recent call last):
  File "<pyshell#361>", line 1, in <module>
    sorted(list1,cmp = lambda x,y : cmp(y[0],x[0]))
TypeError: 'cmp' is an invalid keyword argument for this function
```

由于python3把cmp参数给取消掉了,所以无法使用。


```python
>>> help(sorted)
Help on built-in function sorted in module builtins:

sorted(iterable, key=None, reverse=False)
Return a new list containing all items from the iterable in ascending order.

A custom key function can be supplied to customise the sort order, and the
reverse flag can be set to request the result in descending order.

>>> 
```

注：效率key>cmp(key比cmp快)，所以在python3中把cmp参数给取消了。
