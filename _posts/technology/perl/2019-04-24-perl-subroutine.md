---
layout: post
title: perl子程序
date: 2019-04-24 13:43:23 +0800
categories: 技术文档
tag: Perl
---
* content
{:toc}

# [Perl 子程序(函数)](https://www.cnblogs.com/sunziying/p/9569533.html)	 

### 1、Perl 子程序(函数) ###
Perl 子程序也就是用户定义的函数。
Perl 子程序即执行一个特殊任务的一段分离的代码，它可以使减少重复代码且使程序易读。
Perl 子程序可以出现在程序的任何地方，语法格式如下：

~~~perl
sub subroutine{
   statements;
}
~~~

调用子程序语法格式：
subroutine( 参数列表 );


### 2、向子程序传递参数 ###
Perl 子程序可以和其他编程一样接受多个参数，子程序参数使用特殊数组 @_ 标明。  **（@_ 子程序的私有变量）**
因此子程序第一个参数为 $\_[0], 第二个参数为 $_[1], 以此类推。

~~~shell
Scalar value @_[0] better written as $_[0]
~~~

不论参数是标量型还是数组型的，用户把参数传给子程序时，perl默认按引用的方式调用它们。

~~~perl
#!/usr/bin/perl

use warnings;
use strict;

sub Average {
my $n=@_;
my $sum=0;

foreach (@_) {

 $sum+=$_;

}

my $average=$sum/$n;
print "@_\n";                          #打印整个数组
print "$n\n";                          #打印数组元素的个数
print "$_[0]\n";                       #打印第一个参数
#print "@_[0]\n";  
print "the average is: $average\n"     #打印平均值

}
# 调用函数
Average(12,21,42);

执行以上程序，输出结果为：
12 21 42
3
12
12
the average is: 25

~~~

### 3、向子程序传递数组 ###

由于 @_ 变量是一个数组，所以它可以向子程序中传递列表。
但如果我们需要传入标量和数组参数时，需要把列表放在最后一个参数上。

```perl
#!/usr/bin/perl
 
# 定义函数
sub PrintList{
   my @list = @_;                    #@_  子程序的私有变量
   print "列表为 : @list\n";
}
$a = 10;
@b = (1, 2, 3, 4);
 
# 列表参数
PrintList($a, @b); 　　　　　　　　　　 #列表为 : 10 1 2 3 4
```

注：我们可以向子程序传入多个数组和哈希，但是在传入多个数组和哈希时，会导致丢失独立的标识。所以我们需要使用引用来传递。

### 4、向子程序传递哈希 ###

当向子程序传递哈希表时，它将复制到 @_ 中，哈希表将被展开为键/值组合的列表。

```perl
#!/usr/bin/perl
 
sub PrintHash{
   my (%hash) = @_;
 
   foreach my $key ( keys %hash ){
      my $value = $hash{$key};
      print "$key : $value\n";
   }
}
%hash = ('aa' => 'aa.com', 'bb' => 12138);
 
# 传递哈希
PrintHash(%hash);

以上程序执行输出结果为：
aa : aa.com
bb : 12138
```

### 5、子程序返回值 ###

子程序可以向其他编程语言一样使用 return 语句来返回函数值。
如果没有使用 return 语句，则子程序的最后一行语句将作为返回值。

```perl
#!/usr/bin/perl
 
# 方法定义
sub add_a_b{
   # 不使用 return
   $_[0]+$_[1];  
 
   # 使用 return
   return $_[0]+$_[1];  
}
print add_a_b(1, 2)；        #结果是3
```

### 6、子程序的私有变量 ###

默认情况下，Perl 中所有的变量都是全局变量，这就是说变量在程序的任何地方都可以调用。
如果我们需要设置私有变量，可以使用 my 操作符来设置。
my 操作符用于创建词法作用域变量，通过 my 创建的变量，存活于声明开始的地方，直到闭合作用域的结尾。
闭合作用域指的可以是一对花括号中的区域，可以是一个文件，也可以是一个 if, while, for, foreach, eval字符串。

```perl
#!/usr/bin/perl
 
# 全局变量
$string = "Hello, World!";
 
# 函数定义
sub PrintHello{
   # PrintHello 函数的私有变量
   my $string;
   $string = "Hi!";
   print "函数内字符串：$string\n";
}

# 调用函数
PrintHello();
print "函数外字符串：$string\n";

以上程序执行输出结果为：
函数内字符串：Hi!
函数外字符串：Hello, World!
```

### 7、变量的临时赋值 ###
我们可以使用 local 为全局变量提供临时的值，在退出作用域后将原来的值还回去。
local 定义的变量不存在于主程序中，但存在于该子程序和该子程序调用的子程序中。定义时可以给其赋值。

```perl
#!/usr/bin/perl
 
# 全局变量
$string = "Hello, World!";
 
sub PrintHello1{
   # PrintHello 函数私有变量
   local $string;
   $string = "Hello!";
   print "Print  函数内字符串值：$string\n";
   
   # 子程序调用的子程序
   PrintHello11();
}
sub PrintHello11{
   print "Print 函数内字符串值：$string\n";
}
 
 
sub PrintHello2{
   print "Print 函数内字符串值：$string\n";
}
 
 
# 函数调用
PrintHello1();
PrintHello2();
print "函数外部字符串值：$string\n";


以上程序执行输出结果为：

Print 函数内字符串值：Hello!
Print 函数内字符串值：Hello!
Print 函数内字符串值：Hello, World!
Print 函数内字符串值：Hello, World!
```

### 8、静态变量 ###
state操作符功能类似于C里面的static修饰符，state关键字将局部变量变得持久。
state也是词法变量，所以只在定义该变量的词法作用域中有效。

~~~perl
#!/usr/bin/perl

use feature qw(state);

sub PrintCount{
   state $count = 0;                       # 初始化变量

   print "counter 值为：$count\n";
   $count++;
}

for (1..5){
   PrintCount();
}

以上程序执行输出结果为：
counter 值为：0
counter 值为：1
counter 值为：2
counter 值为：3
counter 值为：4
~~~

