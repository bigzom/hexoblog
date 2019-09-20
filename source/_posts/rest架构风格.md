---
title: rest架构风格
date: 2019-08-27 23:52:52
tags: [rest,前后端分离]
---
##### 简介
1. 在rest概念中每个资源都是一个实体
2. 使用不同的请求表示不同的数据交互方式（put更新 ,get获得 ,pot添加 ,delete删除）
<!--more-->
3. 使前后端分离(用ajax+json的方式进行数据交互)
##### 常用注解
- @RestController
  因为通常使用AJAX+JSON来实现restful架构风格，那么我们需要在每个Controller上面加上@ResponseBody来标识该方法返回值放在响应体中，太麻烦了所以就有该注解代替原来的@Controller，这样就不用每次多写一个注解了
- @RequstBody
  加在javabean前面，可以将请求的值赋值到相应的bean中
- @GetMapping、@PostMapping、@PutMapping、@DeleteMapping
  用来代替RequstMapping分别只处理get、post、put、delete请求
