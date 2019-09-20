---
title: servlet
date: 2019-08-23 10:41:53
tags: [servlet,session,cookie]
---

##### 什么是severlet
 用于开发动态web资源的技术，可以通过HTTP协议接受和响应来自web客户端的请求。
 创建：实现javax.servlet.Servlet接口，该接口在servlet-api.jar包中
 生命周期：实例化 -》初始化 -》 服务 -》 销毁
 ServletContext：所有servlet都访问它，又叫application  
- 多个url-pattern匹配
路径后缀优先原则
精确优先路径原则
最长路径优先匹配原则
##### GenericServlet
- 由于实现severlet 需要实现 Servlet抽象接口，但是实际中我们并不需要用到其全部方法
  所以改为继承GenericServlet重写需要的方法即可
##### HttpServlet（实际开发中常用）
用于处理不同的提交方式（get、post等avax.servlet.Servlet接口无法直接处理）
##### session
- 如何获得session
HttpSession session= request.getSession();
得到会话ID：String id=session.getId();
存数据：session.setAttribute(arg0, arg1);
取数据：session.getAttribute(arg0);
移除数据：session.removeAttribute(arg0);
强制让会话无效：session.invalidate();


- 服务器会为每一个浏览器创建一个session,只能访问自己得session
- http协议是无状态的，那么浏览器是如何识别浏览器的呢
    每个session对象都会交由Map管理，Map会为其生成一个32位的id（JSessionID），响应给浏览器cookie只要浏览器不关闭，下次请求时就会带上该cookie
 在web.xml中设置失效时间(分钟)：<session-config> <session-timeout>60</session-timeout></session-config>
##### cookie
创建cookie对象：Cookie cookie1 = new Cookie("username","lql");
绑定路径：cookie1.setPath(request.getContextPath()+"/aaa"); 只有访问该路径才会添加cookie
设置有效时间：cookie1。setMaxAge(60*60);一小时
将cookie对象添加到响应中：response.addCookie(cookie1);
获取cookie:Cookie cookies = requset.getCookies(); c.getName(); c.getValue();