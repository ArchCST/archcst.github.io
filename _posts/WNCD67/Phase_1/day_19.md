title: Java x D19
date: 2020-01-03
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
包装类：自动装箱和自动拆箱，以及缓存区的坑
Calender 到底是不是单例模式
{% endcq %}
<!--more-->
# 包装类：自动装箱和自动拆箱，以及缓存区的坑
```java
public class LearnPackageClass {
    public static void main(String[] args) {
        // 自动装箱：使用引用数型接收基本数据类型
        Double d3 = 10.1; // 自动调用 Double.valueOf();
        Double d4 = Double.valueOf(10.1);

        // 自动拆箱：
        Double d5 = new Double(200);
        double d6 = d5; // 自动调用 doubleValue(); Number 类中抽象方法的实现
        double d7 = d5.doubleValue();


        // 缓存区范围:
        //     Boolean: true,false
        //     Byte、Short、Integer、Long: -128 ~ 127
        //     Character: 0 ~ 127
        //     Double、Float: 没有

        // 面试题1:
        // 包装类缓存区只适用于自动装箱，不适用于手动装箱
        boolean q1 = new Integer(5) == new Integer(5);
        System.out.println(q1);

        // 面试题2:
        // 范围内自动装箱引用缓存区
        Integer i1 = 10;
        Integer i2 = 10;
        boolean q2_1 = i1 == i2;
        System.out.println(q2_1);
        // 范围外自动装箱区生成新实例
        Integer i3 = 128;
        Integer i4 = 128;
        boolean q2_2 = i3 == i4;
        System.out.println(q2_2);

        // 面试题3:
        // 范围内自动装箱引用缓存区
        boolean q3_1 = Integer.valueOf(5) == Integer.valueOf(5);
        System.out.println(q3_1);
        // 范围外自动装箱区生成新实例
        boolean q3_2 = Integer.valueOf(-129) == Integer.valueOf(-129);
        System.out.println(q3_2);

        // 包装类转字符串
        Integer intX = 200;
        String str1 = String.valueOf(intX);
        String str2 = intX.toString();
        String str3 = intX+"";

        // 字符串转基本数据类型
        int intY = Integer.parseInt(str1);
        double doubleY = Double.parseDouble(str2);
    }
}
```
# Calender 到底是不是单例模式
Calendar 的 getInstance() 并不是单例模式。使用getInstance仅仅是因为Calendar类是一个抽象类无法直接实例化，所以 Calendar 类提供了一个获取子类对象的方法，这个方法名为getInstance()而已。在实际使用时实现特定的子类的对象。
由于Calendar类是抽象类，且Calendar类的构造方法是protected的，所以无法使用Calendar类的构造方法来创建对象，API中提供了getInstance方法用来创建对象。
