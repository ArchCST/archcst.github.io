title: Java x D11
date: 2019-12-26
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
{% endcq %}
<!--more-->
# 抽象类 abstract 的注意事项
抽象类和抽象方法必须用abstract关键字修饰
抽象类不一定有抽象方法，有抽象方法的类一定是抽象类
抽象类不能实例化
抽象类的子类必须襟现所有抽象方法，否则子类只能为抽象类
abstract不能和static final private一起使用
抽象类的子类实现抽象方法时，必须保证方法名、参数列表、返回值相同，访问修饰符只能扩大或相等

# 接口 interface
成员变量都是 public static final 的静态常量
成员方法必须都是抽象方法，不能是普通方法
```java
public interface A {
}

public class B implements A {
}
```
子类必须实现父接口所有抽象方法，或者子类为抽象类

Java是单继多实现的：
```java
public lcass B extends D inplements A,C {
}
```
一个接口可以有多个子类
一个子类可以实现多个接口
接口强调子类必须要有什么（A has B）：通常有来限制子类必须要有什么功能
抽象类强调子类属于抽象类（B is A）：三角形属于平面图形

JDK 1.8 后引入了默认方法和静态方法：
```java
// 默认方法：访问修饰符 public 可以不写
public default void foo(){}
// 静态方法：通过接口名调用
public static void foo(){}
```
多接口实现如果

# 跟打
抽象类：
允许类中存在抽象方法
格式：
```text
访问修饰符 abstract class 类名{
    类内容
}
```

抽象类特点：
抽象类和抽象方法必须用 abstract 关键字修饰
抽象类不能实例化（创建对象）
抽象类中可以没有抽象方法，但有抽象方法的类必须是抽象类或者接口
abstract不能与 static final private 一起使用
抽象类的子类必须实现抽象类中所有抽象方法，否则子类只能为抽象类
子类实现抽象类的抽象方法时，必须保证方法名称、参数列表、返回值相同，且访问修饰符只能扩大或相同

接口 interface
抽象类是从多个类中象出来的模板，如果将这种抽象进行的更彻底，则可以抽象出一种更特殊的抽——接口
```java
/*
访问修饰符 interface 接口名称{
    接口内容
}
*/
public interface A{
    public static final double PI=3.14;
}
// 实现
// 访问修饰符 class 类名 implements 接口1, 接口2... {}
public class B implements A{}
```
特点：
成员变量为静态常量，默认也为 public static final 修饰
JDK 1.8 之前成员方法只能是抽象方法，默认为public abstract 修饰，JDK 1.8新增功能，允许在接口中使用默认方法和静态方法
接口可以被多实现，接口与接口之间可以多继承
