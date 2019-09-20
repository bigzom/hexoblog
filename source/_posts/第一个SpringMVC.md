---
title: 第一个Spring MVC
date: 2019-08-29 10:47:43
tags: [springMVC,参数]
---
#### Spring MVC简介
表现层框架、简化servlet开发（代替servlet）、统一编码风格
<!--more-->
#### 第一个Spring MVC程序 （xml配置方式）
Spring版本：5.0.4、java版本：jdk8+
- 第一步
   创建一个maven的项目，用于管理jar包的导入，以及项目的打包
- 第二步
在pom.xml中导入jar包
  ```xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.4.RELEASE</version>
    </dependency>
  </dependencies>
  ```
- 第三步
在web.xml文件中，注册Spring MVC的中央控制器DispatcherServlet
  ```xml
   <servlet>
        <servlet-name>springMVC</servlet-name>
        <!-- spring MVC中的核心控制器 -->
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
  ```
- 第四步
在该src/main/resources目录下，创建Spring MVC配置文件springmvc.xml
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:p="http://www.springframework.org/schema/p"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
  
  
  </beans>     
  ```
- 第五步
  创建一个类去实现org.springframework.web.servlet.mvc.Controller接口（替代了servlet）
  ```java 
  package com.lql.controller;
  
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  import org.springframework.web.servlet.ModelAndView;
  import org.springframework.web.servlet.mvc.Controller;
  /**
   * @author lql
   * @version 1.0
   * @date 2019/8/29 15:12
   */
  public class SpringController implements Controller {
      @Override
      public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse   httpServletResponse) throws Exception {
          ModelAndView mv = new ModelAndView();
          mv.addObject("hello", "hello first spring mvc");
          mv.setViewName("/WEB-INF/jsp/first.jsp");
          return mv;
      }
  }
  
  ```
- 第六步
  在springmvc.xml配置文件中注册第五步创建的Controller(告诉spring控制器在哪)
  ```xml
  <bean id="/hello.do" class="com.lql.controller.SpringController" />
  ```
- 配置视图解析器

可简化
  ```java
   mv.setViewName("/WEB-INF/jsp/first.jsp");
  ```
打开springmvc.xml的配置文件，在里面添加一个视图解析器：
  ```
    <bean
              class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/jsp/" />
          <property name="suffix" value=".jsp" />
      </bean>
  ```
配置后：
  ```java
   mv.setViewName("first");
  ```

#### 注解方式
- 注册扫描器
  ```xml
      <!-- 注册组件扫描器 -->
      <!-- 会扫描该目录下所有加了Controller注解的类 -->
      <context:component-scan base-package="com.lql.controller"/>
  ```
如果在你的springmvc.xml文件中配置了静态资源即使用了`mvc:resources`
标签的话，那么需要在你的配置文件中配置注解驱动，添加：` <mvc:annotation-driven/>`

- 创建测试类
@Controller() 表示该类为一个Controller
@RequestMapping()   拦截地址
在Controller类上表示命名空间存放共同的url
在方法上存放剩下不同的url
改属性可以设置接收请求的提交方式：`@RequestMapping(value="/test.do",method = RequestMethod.POST)`
- 请求中携带的参数
要求请求中必须携带请求参数 name 与 age
`@RequestMapping(value="/test.do" ,  params={"name" , "age"}) `
要求请求中必须携带请求参数 age，但必须不能携带参数 name
`@RequestMapping(value="/test.do" , params={"!name" , "age"}) `

##### 接收表单提交的参数
在方法参数写上相应的名字即可，第一个参数是参数名不一致的情况
```java
 @RequestMapping("/regist1.do")
    public ModelAndView regist1(@RequestParam(name="username") String t_username, int age) throws Exception{

        ModelAndView mv = new ModelAndView();
        mv.addObject("username", t_username);
        mv.addObject("age", age);
        mv.setViewName("result");
        return mv;
    }
```
pring mvc只会自动为以下五个参数进行自动赋值：
　　　　HttpServletRequest
　　　　HttpServletResponse
　　　　HttpSession
　　　　请求携带的参数
　　　　用于承载数据的Model


使用对象赋值时，如果对象包含另一个对象的引用，
前台名称：school.schoolName（引用名称.引用对象的属性名）

##### 获取请求参数乱码
在web.xml中配置
```xml
<!--字符编码过滤器-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

    <!--指定字符编码-->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>

    <!--强制指定字符编码，即如果在request中指定了字符编码，那么也会为其强制指定当前设置的字符编码-->
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>

</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```