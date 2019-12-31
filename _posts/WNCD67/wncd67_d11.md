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
<span style="font-variant: small-caps;">note</span>
*基本数据类型、数组、字符串之间不同的数据传递*
{% endcq %}
<!--more-->

# 基本数据类型、数组、字符串之间不同的数据传递
```java
public class DataTransfer {
    public static void main(String[] args) {
        DataTransfer dt = new DataTransfer();
        int a = 11;
        // 值传递
        // 把栈里 a 存的基本数据类型 int 的值传递给了 test 的形参
        dt.transferInt(a);
        System.out.println("原变量的值："+a);

        // 分割线
        System.out.println("---------");

        int[] array = new int[]{1};
        // 址传递，实际上也是值传递
        // 把栈里 array 存的地址（引用类型存在栈里的值就是指向堆的地址）传递给了 testPlus 的形参
        dt.transferArray(array);
        System.out.println("原变量的值：" + array[0]);

        // 分割线
        System.out.println("---------");

        String name = "Bruce";
        // 因为字符串是 final 过的 char 数组，所以修改字符串变量实际是给变量换了一个内存地址
        dt.transferString(name);
        System.out.println("原变量的值：" + name);

    }

    public void transferInt(int a){
        System.out.println("int 赋值前：" + a);
        a = 999;
        System.out.println("int 赋值后：" + a);
    }

    public void transferArray(int[] nums) {
        System.out.println("数组赋值前：" + nums[0]);
        nums[0] =999;
        System.out.println("数组赋值后：" + nums[0]);
    }

    public void transferString(String str) {
        System.out.println("字符串赋值前：" + str);
        str ="Bob";
        System.out.println("字符串赋值后：" + str);
    }
}
```
输出结果：
```text
int 赋值前：11
int 赋值后：999
原变量的值：11
---------
数组赋值前：1
数组赋值后：999
原变量的值：999
---------
字符串赋值前：Bruce
字符串赋值后：Bob
原变量的值：Bruce
```