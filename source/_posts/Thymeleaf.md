---
title: Thymeleaf
date: 2019-09-01 22:38:35
tags: [Thymeleaf]
---
##### Thymeleaf简介
Thymeleaf是一个java开发的模板引擎，模板引擎是一个技术名词，是跨领域跨平台的概念。
可以使用它创建经过验证的XML与HTML模板。有利于前后端分离
<!--more-->
##### 第一个thymeleaf程序
- 添加thymeleaf依赖
  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
  </dependency>
  <!--去除thymeleaf的校验 -->
   <dependency>
        <groupId>net.sourceforge.nekohtml</groupId>
        <artifactId>nekohtml</artifactId>
        <version>1.9.22</version>
    </dependency> 
  ```

- 修改spring boot配置文件
关闭thymeleaf的缓存、去除thymeleaf的校验

  ```xml
  spring.thymeleaf.cache=false
  spring.thymeleaf.mode=LEGANCYHTML5
  ```

- 写controller
  ```java
  @Controller
  public class StuController {
      @Autowired
      private StudentService studentService;
  
      @RequestMapping("/getAllStudent")
      public String selectAllStudent(Model model){
          List<Student> list = studentService.selectAllStudent();
          model.addAttribute("students",list);
          return "index";
      }
  }
  ```
- 在resources\templates目录下创建index.html

  这个必须添加:`<html xmlns:th="http://www.thymeleaf.org">`
  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8"/>
      <title>Spring boot集成 Thymeleaf</title>
  </head>
  <body>
  <p th:text="${students}">Spring boot集成 Thymeleaf</p>
  <br/>
  <table>
      <tr th:each="user : ${students}">
          <td th:text="${userStat.index}"></td>
          <td th:text="${user.id}"></td>
          <td th:text="${user.name}"></td>
          <td th:text="${user.age}"></td>
      </tr>
  </table>
  </body>
  </html>
  ```
##### 其它功能
- request：相当于是HttpServletRequest对象
  ```${#request.getContextPath()}```
- session：相当于是HttpSession对象
  ```${#session.getAttribute("phone")}```
- 除了上面的对象之外，工作中常使用的数据类型，如集合，时间，数值，thymeleaf的专门提供了功能性对象来处理它们
