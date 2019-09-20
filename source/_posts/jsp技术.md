---
title: jsp技术
date: 2019-08-24 23:30:13
tags: [jsp,EL表达式,JSTL]
---
##### jsp简介
- jsp是一种动态网页技术
<!--more-->
- 如何实现动态
  网页中可以内嵌java代码，从而实现动态。它就是个servlet
- 作用
  解决在servlet中写html标签带来的不便
##### 如何在jsp中写java代码
- 导包
  写在jsp页面最上方
  `<%@ import.until.*>`
- 写java代码
  `<% java代码 %> `
- 写方法体,不建议用这个
  `<%! 方法体 %>`
  调用方法
  `<% 方法名();`
- 注释
  html注释：`<--注释内容 -->` html后台源码可见
  jsp注释:`<%-- 注释内容 --%>` html后台源码不可见
- 直接在页面显示值
  `<%=要显示的变量或者常量>`
##### jsp工作原理
第一访问jsp页面时调用TomCat的翻译引擎将其翻译成Java文件并编译，其中的类间接的继承了servlet，所以说JSP就是servlet.

##### 九个内置对象
这几个对象Tomcat已经实现，我们可以直接调用
- pageContext(不常用)
 页面上下文，可用通过setAttribute和getAttribute设置和访问只在当前页面有效的数据，不过当前页面的值都是可以直接调用的，所以不常用
- out
 该类型继承了IO流中的Writer,所以是一个输出流，使用上和printWriter类似
- page
 就是当前jsp对象，serlet自己
- exception
通常配合page使用
- application requst response session config 
使用上和在serlet中一样

##### JSP指令
为当前页面赋予属性值，听过运行环境
在JSP中存在三类指令
一种类型可以包含多个属性值
`<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" isErrorPage =true %>`
- page,页面指令
  1. pageEncoding, contentType 设定页面字符编码格式
  2. import 导包，多个以逗号隔开
  3. errorpage 指定异常跳转页面，若想使用exception需开启isErrorPage指令
  4. session属性，默认为开的。指定当前页面可以使用session对象 
- include,包含指令
  将另一个页面的资源包含进来（叫做，静态引入或静态包含）
- taglib,标签指令
##### JSP标签
用于在jsp种大量使用java语句会使页面看着比较凌乱，所以提供一些能实现java代码部分功能的标签，使页面看起更加整洁。
- 常用的两个标签
1.forward
可以转发包含用户reust请求的数据
`<jsp:forward page="Middle.jsp"/>`
2.inclued(动态引入)
`<jsp:include page="left.jsp"/>`
和静态引入的区别在余其在被转化成java文件时，会生成两个。所以他们变量不是共享的。而静态引入是在同一个java文件中所以它们的变量是共享的。优先使用静态引用
##### EL表达式
方便从各大域中获取值(形式：${变量名})，包含4大域pageConext、requst、session、application。<font color=red>只能从这面取</font>
``` java
  <%--从域中获取值 --%>
    <%
      pageContext.setAttribute("name","lql");
      session.setAttribute("sex","male");
      request.setAttribute("age", 25);
      application.setAttribute("hobby","none");
    %>
    ${name}-${sex}-${age}-${hobby}
    </br>
    <%--从Bean对象中获取值 --%>
    <%
      Book book =new Book(1,"C程序设计");
      pageContext.setAttribute("bookName",book);
    %>
    ${bookName.bookname}
    </br>
    <%--从List中获取Bean对象的值--%>
    <%
      Book book2 =new Book(1,"java从入门到精通");
      List list = new ArrayList();
      list.add(book);
      list.add(book2);
      session.setAttribute("bookList",list);
    %>
    ${bookList[1].bookname}
  <br/>
    <%--运算符 --%>
  ${1>2}-${1==2}
    <br/>
  <%--empty运算--%>
    ${empty bookList[1]}-${empty bookList[3]}-${empty asdasd}
  ```
  ```
  结果：
  lql-male-25-none 
  C程序设计 
  java从入门到精通 
  false-false 
  false-true-true
  ```
  ##### EL表达式内置对象
  一共11个，常用：
  - pageContext
  与jsp的内置对象pageContext是同一个，是javax.servlet.jsp.PageContext类型，其它都是java.util.map类型
  - pageScope
  - requstScope
  - sessionScope
  - applicationScope
  - param
    可以获取到请求参数`${param.参数名}`
  - paramValues
    若有多个参数`${paramValues.参数名[下标]}`
  - initParam
    获取web.xml中的初始化参数`${initParam.参数名 }`
- [自定义EL函数](https://www.cnblogs.com/z941030/p/4767712.html)

##### JSTL
JSP标准标签库，可以使用其实现JSP页面逻辑处理,判断、循环等，还提供了对字符串处理的功能
- 使用前准备
  - jar包导入（1.2.x以后）
   taglibs-standard-spec-1.2.5.jar
   taglibs-standard-impl-1.2.5.jar
   taglibs-standard-jstlel-1.2.5.jar
   xalan-2.7.1.jar
   serializer-2.7.1.jar
  - jar包导入（1.2.x以后）
   jstl.jar
   standard.jar
- 初步使用
  ``` js
  //引入对字符处理标签库  
  <%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
  //调用标签库
  ${fn:toUpperCase("lql")}
  ```
- JSTL核心标签库
1.c:out标签：`<c:out value="${user}"></c:out> `
还不如el表达式简洁：`${user}`
2.c:catch标签（显示异常，不常用）
  ```java
  <c:catch var="e">
      ${pageContext.name};
  </c:catch>
  message = ${e.message }
  ```
  3.c:if标签
    ``` java
    <%  
        pageContext.setAttribute("user", "admin");
    %>
    <%--var存放结果。 scope指定存放结果的范围 这两个不常用--%>
    <c:if test="${user == 'admin' }" var="flag" scope="request">
        欢迎登陆
    </c:if>
    ```
  4.c:choose标签
   ```java
   <% 
       pageContext.setAttribute("hobby", "basketball");
   %>
   <c:choose>
       <c:when test="${hobby == 'basketball'}">
           我喜欢打篮球
       </c:when>
       <c:when test="${hobby == 'football'}">
           我喜欢踢足球
       </c:when>
       <c:otherwise>
           我没什么爱好
       </c:otherwise>
   </c:choose>
     
   ```
  5.c:forEach标签:于循环遍历数组、集合（List、Set、Map）,可以指定索引及步长
##### 格式化标签库
可以对日期、数字进行格式化操作
- 引入标签库:`<%@taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>`
fmt:formatDate标签:使用不同的模式格式化日期
fmt:parseDate标签:将指定字符串转换为日期类型
fmt:formatNumber标签:指定格式对数字进行格式化
fmt:parseNumber标签:解析数字
