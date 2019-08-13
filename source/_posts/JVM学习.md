---
title: JVM学习
date: 2019-07-13 16:56:51
tags: [jvm]
description: 类的加载连接与初始化
---
### 类的生命周期
加载： 将字节码文件加载到内存(二进制)
连接
　　　验证：验证字节码的正确性
　　　准备：为静态变量开辟内存，并为其赋予默认值
　　　解析：将类中符号引用变为直接应用
<!-- more -->
初始化：为静态变量赋予指定值
使用
卸载
##### <font color=red>类型的加载、连接、初始化在运行期间完成的</font>
### 程序结束的条件
调用 System.exit()
程序正常结束
程序异常终止
外部系统出错

### 类的使用与初始化
内的使用分为<font color=red >主动使用</font>和 <font color=red>被动使用</font>
##### <font color=blue>首次"主动"使用时才会被初始化</font>
主动使用：
1. 创建一个实例（new一个对象）
2. 对“静态”的操作
　　jvm的助记符：getstatic(访问静态变量)、putstatic(静态变量赋值)、invokestatic(访问静态方法)
3. 反射
4. 初始化了子类，
5. 标为启动类的类  

被动使用：除了上面都是被动（<font color= red>注:被动使用不会被初始化，可能会加载和连接</font>）

主动调用与被动调用示例：
- 当运行 `System.out.println(child1.str1);`时结果为：
```
This is parent1
hello
```
<font color=blue>对于静态字段，只会初始化定义其的类</font>
- 当运行`System.out.println(child1.str2);`时结果为：
```
This is parent1
This is child1
word
```
<font color=blue>当初始化子类时，会先初始化其父类</font>

完整代码如下：
``` java
package jvm.classloder;

public class MyTest1 {
    public static void main(String[] args) {
        System.out.println(child1.str1);
        //System.out.println(child1.str2);
    }
}

class MyParent1{
    public static  String str1 = "hello";
    static {
     System.out.println("This is parent1");
 }
}

class child1 extends MyParent1{
    public static String str2 = "word";
    static {
        System.out.println("This is child1");
    }
}
```
