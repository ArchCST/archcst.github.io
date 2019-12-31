title: Java x D3
date: 2019-12-18
updated: 2019-12-23
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">note</span>
逻辑运算符
字符判断
闰年判断
浮点数的精度问题
基本数据类型的强制转换

<span style="font-variant: small-caps;">homework</span>
用户登录 V1
`System.out.println((byte)128)` 输出什么，为什么？
`System.out.println((int)(char)(byte)-1)` 输出什么，为什么？
不用第三个数交换两个数的方法
{% endcq %}
<!--more-->

# 课堂笔记
## 逻辑运算符
与：&& and （一个 false 则为 false）
或：|| or  （一个 true 则为 true）
非：! not （取反）

```java
public static void logicalOperations() {
    Scanner sc = new Scanner(System.in);
    System.out.print("请输入一个字符：");
    int score = sc.nextInt();
    if (score >= 60 && score < 90) {
        System.out.println("你的分数是" + score + "，平行班！");
    } else if (score > 90 || score == 0) {
        System.out.println("你的分数是" + score + "，火箭班！");
    } else {
        System.out.println("你的分数是" + score + "，回炉重造！");
    }
}
```

## 字符判断
通过 ASCII 码判断用户输入的字符是什么字符
```java
public static void isIdentifier() {
    Scanner sc = new Scanner(System.in);
    System.out.print("请输入一个字符：");
    int chInt = sc.next().charAt(0);
    if (chInt > 47 && chInt < 58) {
        System.out.println("这是一个数字，数字是标识符");
    } else if ((chInt >= 65 && chInt <= 90) || (chInt >= 97 && chInt <= 122)) {
        System.out.println("这是一个字母，字母是标识符");
    } else if (chInt == 95 || chInt == 36) {
        System.out.println("这是一个标识符");
    } else {
        System.out.println("这不是一个标识符");
        System.out.println("字符的 ASCII 码为：" + chInt);
    }
}
```

## 闰年判断
```java
public static void leapYear() {
    Scanner sc = new Scanner(System.in);
    System.out.print("请输入任意年份，判断是不是闰年：");
    int year = sc.nextInt();
    if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
        System.out.println("是闰年！");
    } else {
        System.out.println("不是闰年！");
    }
}
```

## 浮点数的精度问题
`1.10` 换算成二进制为 `1.00011001100110011...` 无限循环，会丢失掉尾数
```java
double a = 2.00;
double b = 1.10;
System.out.println(a - b);
```

## 基本数据类型的强制转换
为什么 d = (byte) 300 结果为 44 ？
因为 int 300 = (0000 0001 0010 1100)，而 byte 只有 (0000 0000)，所以截取了后八位 (0010 1100)，结果为 44
```java
int c = 300;
byte d = (byte) c;
System.out.println(d);
```

# Homework
## 用户登录 V1
控制台提示用户输入账号、密码
分别判断账号和密码
```java
public static void login() {
    // Setup right account and password
    String rightName = "admin";
    String rightPass = "123456";

    // Read account from console
    Scanner sc = new Scanner(System.in);
    System.out.print("Account: ");
    String inputName = sc.next();

    // Validate account
    if (!inputName.equals(rightName)) {
        System.out.println("No account: " + inputName);
    } else {
        // Read password from console
        System.out.print("Password: ");
        String inputPass = sc.next();

        // Validate password
        if (!inputPass.equals(rightPass)) {
            System.out.println("Wrong password!");
        } else {
            System.out.println("Success!");
        }
    }
}
```
## System.out.println((byte)128) 输出什么，为什么？
```
1. 正数的 反码 和 原码 相同
2. 正数的 补码 和 原码 也相同
3. 负数的 反码 符号位不变其余的按位取反
4. 负数的 补码 则是其反码+1
5. 补码的补码就是原码
 
   (int) 128 = [0000 0000 0000 0000 0000 0000 1000 0000]原
正数补码不变 = [0000 0000 0000 0000 0000 0000 1000 0000]补
强转 byte 丢弃高位              (byte) 128 = [1000 0000]补

System.out.println((byte)128); --> " + (byte) 128);
内存中就是 [1000 0000]补，但人读要转原码，计算过程如下：
补码的补码即为原码，负数的补码符号位不变其余位按位取反后+1：
[(1)000 0000]补 = [(1)111 1111]反 = [(1) 1000 0000]原 = 十进制 -128
 (1)为不参与计算的符号位 (负号)

[(1) 1000 0000]原 只是方便人读的原码，并非内存中的值，内存中依旧是 [1000 0000]补，并没有第9位。
这也是为什么 byte 负数边界的绝对值（128）比正数边界（127）大1的另一个解释。

Java 在处理 byte、short、char 时会转换为 int 来操作，并分配 4 Byte 内存
  (byte) 128 = [1111 1111 1111 1111 1111 1111 1000 0000]补 = -128
```

## System.out.println((int)(char)(byte)-1) 的输出什么，为什么？
```
 (byte) -1  =  [1000 0001]原  =  [1111 1110]反  =  [1111 1111]补
转 char，高位补符号位 (char) (byte) -1 = [1111 1111 1111 1111]补
由于 char 取值范围是 0 ~ 65535，故无符号位，转 int 高位补0：
(int) (char) (byte) -1 = [0000 0000 0000 0000 1111 1111 1111 1111]补
                       = [0000 0000 0000 0000 1111 1111 1111 1111]原 = 十进制 65535
System.out.println((int) (char) (byte) -1); --> " + (int) (char) (byte) -1
```
# 小技巧
## 不用第三个数交换两个数的方法
```java
    public void swapAB() {
        int a = 7;
        int b = 3;
        System.out.println("a = " + a);
        System.out.println("b = " + b);
        a ^= b;
        b ^= a;
        a ^= b;
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
```
