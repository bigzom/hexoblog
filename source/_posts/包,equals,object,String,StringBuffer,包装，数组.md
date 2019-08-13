---
title: 包,equals,object,String,StringBuffer,包装，数组
date: 2019-08-02 11:27:12
tags: [包,equals,object,String,StringBuffer,包装,数组]
---
##### 包
存放不同功能类的文件夹
命名规则：公司域名反转（小写）
<!-- more -->
##### equals方法
<font color=red>在Object类当中比较的是地址
String类重写了equals方法,比较的是值</font>
- instanceof 
左边是对象，右边是类；当对象是右边类或子类所创建对象时，返回true；否则，返回false
null用instanceof跟任何类型比较时都是false 
##### ==
对于原生类型比较的是值（原生类型没有equals方法）
对于引用类型比较的是地址
##### object
每个类都会显示或者隐式的继承Object这个类
所以每个类都有tostring（）方法，打印对象和打印 对象.tostring 效果是一样的
##### String
字符串是一个常量，当生成便无法改变。(当使用 + 连接字符串时 会生成新的对象)
- StringPool(字符串池)
对字符串直接（字面量）赋值时，首先会检查字符串池是否有该对象，没有则创建，有则直接返回给该对象。
`String name="wang"`
如果是new 出来的字符串则会在字符串池中检查是否有该对象，无则在其中创建，有则跳过，然后在堆中再创建一个该对象并返回堆中的地址。（会在两个地方创建，返回堆中的地址）
`String name = new String(wang)`
##### StringBuffer 
StringBuffer可变字符串，用append方法添加，取出时用toString方法
##### 包装类型
八种基本类型的包装类型是：
Byte、Short、Integer、Long、Flot、Double、Character、Boolean
```java
int a =10;
//转化成包装类型
Integer integer = new Interger(a);
//转化成基本类型
int c = integer.intValue();
```
##### 数组
相同数据类型的集合就叫做数组。
type[] 数组名 = new type[数组长度]
数组长度一旦确定就不能改变
数组里面装的是引用不是对象
##### 二维数组
一种平面二维结构的数组（其实就是数组的数组）
type[][] 数组名 = new type[行长度][列长度]
##### Arrays 类
Arrays类位于 java.util 包中，主要包含了操纵数组的各种方法（排序、填充、比较）
