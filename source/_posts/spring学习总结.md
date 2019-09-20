---
title: spring学习总结
date: 2019-08-31 12:46:47
tags: [spring]
---

- springMVC、mybatis、spring
springMVC用于代替servlet，对请求进行控制转发等
mybatis数据库框架
spring对实体类进行操作：IOC、DI、AOP等，主要作用解耦，降低复杂性，springmvc依赖spring
<!--more-->
- springAOP
1.JDK动态代理：需实现某个接口，耦合度好
2.CHLIB动态代理：性能强
- AOP术语
目标对象：需被增强的对象
切面： 非业务逻辑
连接点：可以被切面织入的方法
切入点：切面具体织入的方法
通知：通知切入的时间
- AOP实现
1.导入aspectj的jar包
2.定义切面
3.配置XML：注册bean,配置切入点、切入面