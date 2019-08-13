---
title: java多态,抽象,接口，单例模式
date: 2019-07-31 15:57:32
tags: [java,多态,抽象,接口,static,final,单例模式]
---
##### 多态
父类对象的引用可以指向子类对象
<!-- more -->
Parent p = new Child(), 当调用子类方法时，父类和子类同时拥有相同方法时才能调用。
<font color=red>指向谁就是调用谁的方法</font>，指向谁才能转换成谁
向上自动转型（子类就是父类），向下强制转型
- 多态好处
多个子类需要通过同一套方法处理时，可以只写一次这个方法，减少代码量
##### 抽象类、抽象方法
``` java
public abstract class AbstractTest{
    public abstract void cry();
}
```
抽象类：用abstract修饰的类
抽象方法：用abstract修饰的方法，但是不会实现方法（有声明，无实现，有 {} 就是实现），定义在抽象类中
抽象类不能被实例化
<font color=red>子类必须实现父类所有抽象方法</font>（除非子类也是抽象类）
- 作用
由于抽象类抽象方法并不具体实现，其作用是规范子类的行为
##### 接口
接口中所有方法都是抽象方法
java中只能继承一个类，但是可以实现多个接口中间用 "," 隔开
接口中可以有成员变量（都是终态 ，默认public final static）
``` java
public interface InterfaceTest{
    //abstract可省略
    public void abstract output();
    public void  input();
}
```
- 类可以实现接口
```java
public  class implements InterInterfaceTest{
    public void output(){
        system.out.println("输出")
    }
        public void input(){
        system.out.println("输入")
    }
}
```
- 作用
与抽象类相似，提供一套规范
- 与抽象类差别
接口是特殊（严格）的抽象类，其中必须全是抽象方法，抽象类则可以存在具体的方法
##### static
修饰成员变量：表示一个类不管生成多少个实例，生成所有实例都访问这一个成员变量
如果一个成员变量是静态的，我们可以通过 类名.成员变量名 来访问它
<font color=red>子类没法重写父类的静态方法，只能继承</font>。（父类的静态方法在子类中被隐藏了）
<font color=red>被隐藏的方法，取决于引用的类型，就调用谁的方法</font>
![avtor](/img/2.png)
![avtor](/img/3.png)
- 静态代码块
加载到JVM上时就执行了（所以只会执行一次），构造方法是生成对象时执行的，所以静态代码块先执行
作用是完成些初始化工作
<font color=red>静态的只能访问静态的，非静态的可以访问一切</font>
不能再静态方法中使用this关键字
##### final
final可修饰类、属性、方法（表示终态）
final修饰类：终态类不能被继承
final修饰方法：不能被重写
final修饰属性：属性不能被重新赋值（指向地址不变就不算重新赋值）
一个类不能既是final又是abstract
- 对final 修饰的对象赋值
1.在声明时赋值
2.在该类所有的构造方法中对该属性赋值
##### 单例模式
一种设计模式，设计的思想，大量依赖多态
一个类只能生成一个对象
```java
public class STest{
public static void main(String[] args){
   SingetonTest sg1 = SingetonTest.getInstance();
   SingetonTest sg2 = SingetonTest.getInstance();
   System.out.println(sg1 == sg2);
}
}
class SingetonTest{
    private static Singetontest sgtest = new Singetontest()
    private singetontest(){
    }
    public static Singetontest getInstance(){
        return sgtest;
    }
}
```