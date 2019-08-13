---
title: Java反射
date: 2019-07-14 11:42:45
tags: [java, 反射]
---
### JAVA反射（运行时）
动态获取类信息、动态调用对象方法
1. 判断一个对象所属的类
2. 构造任意一个类的对象
3. 判断任意一个类所具有的成员变量和方法
4. 调用任意一个对象和方法
<!-- more -->
### 反射的好处
编译器将.java文件编译成.class文件之后，普通方式创建的对象就不能再变了，我只能选择是运行还是不运行这个.class文件。是不是感觉很僵硬，假如现在我有个写好的程序已经放在了服务器上，每天供人家来访问，这时候Mysql数据库宕掉了，改用Oracle,这时候该怎么怎么办呢？假如没有反射的话，我们是不是得修改代码，将Mysql驱动改为Oracle驱动，重新编译运行，再放到服务器上。是不是很麻烦，还影响用户的访问。
假如我们使用反射Class.forName()来加载驱动，只需要修改配置文件就可以动态加载这个类，Class.forName()生成的结果在编译时是不可知的，只有在运行的时候才能加载这个类，换句话说，此时我们是不需要将程序停下来，只需要修改配置文件里面的信息就可以了。这样当有用户在浏览器访问这个网站时，都不会感觉到服务器程序程序发生了变化。
[摘自bihang的博客](https://www.cnblogs.com/bihanghang/p/9992237.html "Java反射的好处")

### 通过反射调用类的方法
1. 获取class对象
第一种：通过Class.forName 静态方法获取
该方法需要知道完整的包路径
`Class classtype = Class.forName("com.lql.reflextest.ReflexTest1");`
第二种：使用.class方法获取
已明确知道有该类
`Class classtype = ReflexTest1.class;`
第三种: 使用类对象的 getClass() 方法(<font color=red>前提是该对象已实例化</font>)
`Class classtype = reflexTest1.getClass();`
- <font color=red>对类进行实例化</font>
`Object invoketest = classtype.newInstance();`
也可用new 来实例化对象
2. 获取到对象的方法 getMethod()
- add:需调用类的方法
- int.class： 指定参数
需要上面两个参数的原因是可以保证唯一性，因为方法的重载，可以有相同的方法名
`Method  method1 = classtype.getMethod("add" ,int.class,int.class);`
3. 调用方法 
- reflexTest1：实例化的对象
- 1，2：传入调用方法的参数
`Object result1 = method1.invoke(reflexTest1,1,2);`

### 完整示例代码
``` java
package com.lql.reflextest;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflexTest1 {
    public int add(int param1, int param2) {
        return param1 + param2;
    }

    public String say(String param1) {
        return "say:" + param1;
    }
}

/**
 * 第一种获取类的方式
 */
class TestReflex1{
    public static void main(String[] args) {
        ReflexTest1 reflexTest1 = new ReflexTest1();
        Class classtype = null;
        try {
            classtype = Class.forName("com.lql.reflextest.ReflexTest1");
        } catch (ClassNotFoundException e) {
            System.out.println("没有发现到该类！");
            e.printStackTrace();
        }
        Method method1 = null;
        Method method2 = null;
        try {
            method1 = classtype.getMethod("add" ,int.class,int.class);
            method2 = classtype.getMethod("say" ,String.class);
        } catch (NoSuchMethodException e) {
            System.out.println("没有该方法！");
            e.printStackTrace();
        }
        Object result1 = 0;
        Object result2 = 0;
        try {
            //注意这里传的时实例化的对象
            result1 = method1.invoke(reflexTest1,1,2);
            result2 = method2.invoke(reflexTest1,"hello");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }

        System.out.println(result1);
        System.out.println(result2);

    }
}

/**
 *第二种获取类的方式
 */
class TestReflex2{
    public static void main(String[] args){
        Class classtype = ReflexTest1.class;
        Object invoketest =null;
        try {
            invoketest = classtype.newInstance();
        } catch (InstantiationException e) {
            System.out.println("实例化失败啦：不能是抽象类、接口、数组类、基本类型或 void； " +
                    "或者该类没有 null 构造方法； 或者由于其他某种原因导致实例化失败");
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            System.out.println("实例化失败啦：该类或其 null 构造方法是不可访问的");
            e.printStackTrace();
        }
        Method method1 = null;
        try {
            method1 = classtype.getMethod("add",int.class,int.class);
        } catch (NoSuchMethodException e) {
            System.out.println("没有该方法");
            e.printStackTrace();
        }
        Object result = null;
        try {
            result = method1.invoke(invoketest,1,3);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println(result);
    }
}
```