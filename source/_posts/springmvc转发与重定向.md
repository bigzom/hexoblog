---
title: springmvc转发与重定向
date: 2019-08-29 18:00:21
tags: [spring]
---
##### 转发与重定向
spring mvc底层其实就是一个servlet，因此在spring mvc中也存在转发和重定向的概念。对于转发的页面，可以是在WEB-INF目录下的页面；而重定向的页面，是不能在WEB-INF目录下的。因为重定向相当于用户再次发出一次请求，而用户是不能直接访问WEB-INF目录下的资源的。
<!--more-->
- 返回ModelAndView时的请求转发
  ```java
    ModelAndView mv = new ModelAndView();
    mv.addObject("type", "被跳转的controller");
    //手动显式指定使用转发，此时springmvc.xml配置文件中的视图解析器将会失效,需写全路径
    mv.setViewName("forward:/jsp/result.jsp");
    return mv;
  ```
使用转发跳转到其他controller中
  ```java
   ModelAndView mv = new ModelAndView();
  
      //手动显式指定使用转发，此时springmvc.xml配置文件中的视图解析器将会失效
      mv.setViewName("forward:other.do");
      return mv;
  ```
- 返回ModelAndView时的重定向
 `mv.setViewName("redirect:other.do");`
- 返回String类型的转发
在controller中的方法参数会被自动的放到request域中。
`return "forward:/jsp/result.jsp";`
result.jsp中直接通过request域获取数据，以下两种方式均可。
  ```
  ${requestScope.school.schoolName}
  ${school.schoolName}
  ```
- 返回String类型的重定向
传递数据需Model实现，数据会放在url中，所以只能传递基本数据类型和String类型
  ```java
  @RequestMapping("/redirectStr.do")
  public String redirectStr(School school, Model model)throws Exception{
  
      //这里的数据同样会放在url中，所以只能传递基本数据类型和String类型
      model.addAttribute("schoolName", school.getSchoolName());
      model.addAttribute("address", school.getAddress());
  
      return "redirect:/jsp/result.jsp";
  }
  ```
result.jsp中需要通过param来获取数据：
  ```
  ${param.schoolName}
  ${param.address}
  ```
- 返回void的重定向和转发

转发：`request.getRequestDispatcher("/jsp/result.jsp").forward(request, response);`
重定向：`response.sendRedirect(request.getContextPath()+"/jsp/result.jsp");` 
