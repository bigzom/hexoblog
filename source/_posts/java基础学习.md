---
title: java基础学习
date: 2019-07-11 17:04:07
tags: [java]
description: java基础学习
---

### 三元运算
语法：逻辑表达式1？ 表达式2：表达式3
<!-- more-->
```
int scoreA = 70;
char degreeA = '优';
char degreeB = '良';
char degreeC = '差';
char c = scoreA >= 80? degreeA:(scoreA >= 60? degreeB:degreeC);
System.out.println(c);
```
### java内存划分
栈：方法局部变量
堆：对象实例
方法区：类信息、代码、静态方法、静态变量、常量

## 数据初始化
静态初始化：赋予了特定值，系统分配长度
动态初始化：指定了长度，没有指定值。不同类型在内存中默认值不一样 int:0 string:null