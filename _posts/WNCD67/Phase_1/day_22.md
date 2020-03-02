title: Java x D22
date: 2020-01-06
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">tool</span>
Timer 运行效率观查
{% endcq %}
<!--more-->
# Timer: 运行效率观查
写了个运行效率观查的工具，都是类变量和类方法，方便调用，不定时更新，[GitHub链接](https://github.com/ArchCST/LearnJava/blob/master/playground/Timer.java)
```java
public class Timer {
    private static long staticStartTime = 0;

    public static void start() {
        if (staticStartTime == 0) {
            System.out.println("--- LOG: Start timing.");
        } else {
            System.out.println("--- WARNING: Reset start time!");
        }
        staticStartTime = System.currentTimeMillis();
    }

    public static void end() {
        if (staticStartTime == 0) {
            System.out.println("--- WRONG: No start time!");
        } else {
            System.out.println("--- Result: "+(System.currentTimeMillis() - staticStartTime)+" ms");
            staticStartTime = 0;
        }
    }
}
```
