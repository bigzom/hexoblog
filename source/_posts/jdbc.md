---
title: jdbc
date: 2019-08-22 12:11:12
tags: [数据库]
---
##### jdbc
java程序与数据库的桥梁
##### 四个核心接口
- DriverManager:用于注册驱动，创建符合该驱动的数据连接
- Connection: 表示与数据库连接的对象，一个Connection对应一个会话
- Statement:操作数据库sql语句的对象，包含2个实现类：Statement 和 PreparedStatement（常用）
- ResultSet:查询结果集

##### jdbc连接实战
- 不推荐的方法实现 Statement
  ```java
   @Test
      public void jdbcTest() throws ClassNotFoundException {
          //注册驱动
          Class.forName("com.mysql.jdbc.Driver");
  
          //java jdk1.7 新特性自动关闭资源，try(需关闭的资源){}，继承了AutoCloseable 的类都可以被关闭
          try (
                  //创建连接
                  Connection connect = DriverManager.getConnection  ("jdbc:mysql://localhost:3306/login", "root", "1234");
                  //得到执行sql的statement对象
                  Statement satement = connect.createStatement();
                  //执行查询语句
                  ResultSet result = satement.executeQuery("select * from student")) {
  
              while (result.next()) {
                  //可用数据库中下标获取或者字段
                  Object id = result.getObject("sid");
                  Object name = result.getObject("sname");
                  Object sex = result.getObject("sex");
                  Object age = result.getObject("age");
                  System.out.println(String.valueOf(id) + " " + name + " " + sex + " " + age);
              }
              //实现增删改都用 executeUpdate
              satement.executeUpdate("insert into student(sname,sex,age) values ('小gg','男',22)");
          } catch (SQLException e) {
              e.printStackTrace();
          }
      }
  
  ```
   __<font color = red>以上代码中 Statement 会存在sql注入问题</font>__
   用PreparedStatement 代替 Statement 可以解决,改进后代码
   ``` java
     @Test
    public void jdbcTest() throws ClassNotFoundException {
        //注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        //java jdk1.7 新特性自动关闭资源，try(需关闭的资源){}，继承了AutoCloseable 的类都可以被关闭
        try (
                //创建连接
                Connection connect = DriverManager.getConnection("jdbc:mysql://localhost:3306/login", "root", "1234");
                //得到执行sql的statement对象，待查语句用 ？ 占位
                PreparedStatement satement = connect.prepareStatement("select * from student where sid = ?")
        ) {
            //为占位符赋值
            satement.setString(1, "10");
            //执行查询语句
            try (
                    ResultSet result = satement.executeQuery()
            ) {
                while (result.next()) {
                    //可用数据库中下标获取或者字段
                    Object id = result.getObject("sid");
                    Object name = result.getObject("sname");
                    Object sex = result.getObject("sex");
                    Object age = result.getObject("age");
                    System.out.println(String.valueOf(id) + " " + name + " " + sex + " " + age);
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
   ```