---
title: JAVA异常学习
date: 2019-07-24 22:32:16
tags: [java,异常]
---
### 异常分类
error: 无法恢复，只能重启程序 --OutfoMemoryEorr
一般性异常:必须处理的异常，程序会报错
RuntimeException:不用显示的处理，例如被0除异常
<!-- more -->
![avator](/img/exception.png)
### 处理方式
- 自己处理
- 交给jvm处理
### 自写异常类
参照原有的异常类，可以很简单的写出来，就几个构造方法，和一个序列化ID。
```java
public class IllegelNameException extends Exception {

    private static final long serialVersionUID = -6710966889073914980L;

    public IllegelNameException() {
        super();
    }

    public IllegelNameException(String message) {
        super(message);
    }
}
```
[序列化ID在IDea插入方法](https://blog.csdn.net/weixin_40836179/article/details/81435203)
##### 调用自写异常类
```java
public class UserLogin {
    //throws 将异常交给调用者处理，类用
    public void Login(String name) throws IllegelNameException {
        if(name.length()<6)
        {
            //thow抛出的是个对象，方法体里面用
            throw new IllegelNameException("用户名长度小于6");
        }
        System.out.println("登录成功");
    }
}
```
##### 测试类
```java
public class LoginTest {
    public static void main(String[] args){
        UserLogin userLogin = new UserLogin();
        //捕获异常
        try {
            userLogin.Login("lql");
        } catch (IllegelNameException e) {
            e.printStackTrace();
        }
    }
}

```

### final、finally、finalize区别
这几个没啥联系就是长得像
- Final
  - 修饰类：不能被继承
  - 修饰变量：只能赋值一次
  - 修饰方法：不能被继承
- finally
  try语句体，finally中语句一定会被执行。
- finalize 
  Object中的方法，在对象没有引用指向时，由对象的垃圾回收器在回收之前调用的方法