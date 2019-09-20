---
title: java8新特新
date: 2019-08-20 08:38:42
tags: [java8,lambda]
---

##### 函数式接口
一个接口中只有一个抽象方法，可以用@FunctionalInterface注解定义
<!-- more-->
##### default方法
用default修饰的方法可以打破接口中只能是抽象方法
目的是增加兼容性，比如想更新某个接口，如果新增加一个抽象方法，其它实现它的方法都需要重写该方法。新增default方法后就不需要重写
变相实现了多继承

##### lambda表达式
<font color=red>可以看作一个匿名的方法，只能用于函数式接口</font>
(参数类型 参数,......) ->{
    代码块；
    //可不写，有的话可以自动返回，没有就不返回
    return 结果；
}
优点：可编写出比匿名内部类更简洁的代码
- 自定义一个函数接口，并用lambda表达式实现

##### 例子
``` java
public class FunctionalProgram {
    public static void main(String[] args) {
        //匿名内部类实现，不用lambda实现
        FunctionalAdd fa = new FunctionalAdd() {
            @Override
            public int add(int a, int b) {
                return a+b;
            }
        };
        System.out.println(fa.add(1,2));

        //使用lambda实现,这里如果只有一句可以不要{}和return
        FunctionalAdd fal = (a,b) -> a+b;
        System.out.println(fal.add(2,3));

        System.out.println("--------------------------lambda遍历集合");
        List list = new ArrayList();
        list.add("a");
        list.add("b");

        //增强for循环遍历集合
        System.out.println("----增强for");
        for(Object i:list){
            System.out.println((String)i);
        }
        System.out.println("----迭代器");
        //迭代器遍历集合
        Iterator itera = list.iterator();
        while (itera.hasNext()){
            System.out.println(itera.next());
        }
        //使用forEach遍历集合
        System.out.println("----foreach");
        list.forEach(new Consumer() {
            @Override
            public void accept(Object o) {
                System.out.println(o);
            }
        });
        //使用lambda，只有一个参数时可以省略()只保留 i 即可
        System.out.println("----foreach-lambda");
        list.forEach(i-> System.out.println(i));
        //lambda还可以精简，只有一条语句时还可以用方法引用
        System.out.println("----超级精简，方法引用");
        list.forEach( System.out :: println);


        System.out.println("--------------------------lambda实现多线程的创建");
        //匿名内部类实现
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("未使用lambda");
            }
        }).start();

        //使用lambda实现
        new Thread(()-> System.out.println("使用了lambda")).start();
        
    }

}
```
```html
输结果：
3
5
--------------------------lambda遍历集合
----增强for
a
b
----迭代器
a
b
----foreach
a
b
----foreach-lambda
a
b
----超级精简，方法引用
a
b
--------------------------lambda实现多线程的创建
未使用lambda
使用了lambda
```