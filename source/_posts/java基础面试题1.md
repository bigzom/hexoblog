---
title: java基础面试题一
date: 2019-09-07 18:13:53
tags: [题]
---
- 什么是设计模式？常用的设计模式有哪些
解决特定问题的设计方法
<!--more-->
单例模式（饱汉模式、饿汉模式）：
　1.构造方法私有化
　2.在本类中创建一个单实例（<font color=red>饱汉是初始化的时候就创建。饿汉是调用时再创建</font>）
　3.提供获取该实例的方法（<font color=red>创建时需方法同步</font>）
工厂模式：spring的IOC就是将对象的创建交给工厂
代理模式：spring的AOP使用就是动态代理
装饰模式：I/O流用得多
---
- http get和post的区别
它们都是http的请求方式，通过不同的请求方式可以完成对资源的不同操作get(查)、post(增)、delet(删)、put(更)
1.get请求的数据会在地址栏中显示出来，多个数据以&分割，安全性差。同时由于地址长度的限制，所以传输大小也会有限制
2.post传输的数据在包中，所以不会显示出来
---
- 什么是servlet?
Java编写的服务端程序。主要功能是前端页面和后台服务的数据交互，生成动态web内容。
自定义servlet需实现servlet接口
---
- servlet生命周期
加载：加载servlet
实例化：服务器实例化servlet
初始化：实例化完成后调用servlet的init方法完成初始化
处理请求：响应请求
销毁：调用其destroy方法
--- 
- servlet API中forward() 与 redirect()区别
1.forword是服务端的转向客户端只需发起一次请求而 redirect是客户端的跳转需发出两次请求，forword效率高
2.forward地址栏不会改变而redirect会改变
---
- jsp和servlet的相同点和不同点
jsp最终都会翻译成继承了hettpservlet的类，所以其是个特殊的servlet
jsp的java代码嵌套在html中侧重于视图，而servlet的应用逻辑在java文件中，完全于html分离，侧重于控制逻辑