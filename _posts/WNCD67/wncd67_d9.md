title: Java x D9
date: 2019-12-24
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
｡* ﾟ+.*.C h rͫ iͤ sͬ tͬ m͗ a s E v e+..｡ * ﾟ+
IntelliJ IDEA 教程链接
IntelliJ IDEA 键位修改
Java 设计模式  /  随性收集中...
Java 相关英语单词  /  随性收集中...
{% endcq %}
<!--more-->
# Youtube 上的 IntelliJ IDEA 的教程
看了很多讲到 Java 源码阅读的文章都还是首推 IDEA，于是开始学习 IDEA 的用法。预计本周内学完。
[IntelliJ IDEA 教程](https://www.youtube.com/watch?v=yefmcX57Eyg)

# IntelliJ IDEA 键位修改
使用了 IntelliJ IDEA Classic 作为 keymap base ，在此基础上简单加了一些 emacs 的移动键，尽量不影响原本的键位设计。
cmd+n:Down
cmd+p:Up
cmd+b:Left
cmd+f:Right
cmd+a:Move Caret To Line Start
cmd+e:Move Caret To Line End

# Java 设计模式  /  随性收集中...
## 开闭原则
1. 一个软件实体如类、模块和方法应该对扩展开放，对修改关闭。
2. 打开应用接口，关闭细节的实现

# Java 相关英语单词
## 编程基础
方法重载：overload
编译：compile

## 面向对象
类：class
对象：object
实例：instance
实例化：instantiate
成员变量：member-variables
实例变量：instance-variable
成员方法：member-function
封装：encapsulation
继承：inheritance
多态：polymorphism
重写：override

# 值传递和址传递
```java
public class DataTransfer {
    public static void main(String[] args) {
        DataTransfer dt = new DataTransfer();
        int a = 11;
        // 值传递
        // 把栈里 a 存的基本数据类型 int 的值传递给了 test 的形参
        dt.test(a);
        System.out.println("原变量的值："+a);

        // 分割线
        System.out.println("---------");

        int[] array = new int[]{1};
        // 址传递，实际上也是值传递
        // 把栈里 array 存的地址（引用类型存在栈里的值就是指向堆的地址）传递给了 testPlus 的形参
        dt.testPlus(array);
        System.out.println("原变量的值：" + array[0]);

    }

    public void test(int a){
        System.out.println("int 赋值前：" + a);
        a = 999;
        System.out.println("int 赋值后：" + a);
    }

    public void testPlus(int[] nums) {
        System.out.println("数组赋值前：" + nums[0]);
        nums[0] =999;
        System.out.println("数组赋值后：" + nums[0]);
    }
}
```
输出为：
```text
int 赋值前：11
int 赋值后：999
原变量的值：11
---------
数组赋值前：1
数组赋值后：999
原变量的值：999
```