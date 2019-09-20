---
title: spring
date: 2019-08-28 18:12:31
tags: [spring]
---
##### spring优点
- 方便解耦，简化开发（高内聚低耦合），可以将对象依赖关系的维护交给Spring管理。
<!--more-->
- <font color =red>IOC（Inversion of Control）控制反转，对象的创建由spring完成，并将创建好的对象注入给使用者。
- AOP（Aspect Orient Programming）编程的支持，面向切面编程，可以将一些日志，事务等操作从业务逻辑的代码中抽取出来，这样子业务逻辑代码就更加纯净了，并且可以增强日志和事务复用性。</font>
- 声明式事务的支持，只需要通过配置就可以完成对事务的管理，而无需手动编程。
- 内部提供了对很多优秀框架的直接支持
- 非侵入式，spring的api不会在业务逻辑的代码中出现，方便移植

##### IOC简介
控制反转（IoC，Inversion of Control），是一种思想。指的是将创建对象的操作权交给容器
- 实现方式
1. 赖注入(Dependency Injection，简称DI)
 若需要调用另一个对象协助时，无须在代码中创建被调用者，而是依赖于外部容器，由外部容器创建后传递给程序。
2. 依赖查找，（Dependency Lookup，DL）

##### 接口
ApplicationContext接口
BeanFactory接口：使用的时候才会创建bean的对象
需要在一些小内存或者资源受限的机器运行程序，这个时候可以选择使用BeanFactory，此时事务的管理、AOP功能将会失效。因此，大部分情况下，建议使用ApplicationContext作为容器

##### bean的装配
spring ioc容器将bean对象创建好并传递给使用者的过程叫做bean的装配。
- bean的三种实例化方式
1. 默认方式（需要一个无参的构造方法）
2. 实例工厂
3. 静态工厂

##### 依赖注入
对对象属性赋值
1. setter方法注入
2. 构造方法注入

