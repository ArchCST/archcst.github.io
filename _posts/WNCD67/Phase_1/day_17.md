title: Java x D17
date: 2020-01-01
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
和谐字符串
分析一个日期是否为闰年，该月有几天，该日是星期几
计算出生日期至今有多少天


{% endcq %}
<!--more-->
# 和谐字符串
```java
/*
在网络程序中，如聊天室、聊天软件等，经常需要对一些用户所提交的聊天内容中的敏感性词语进行过滤。
如“性”、“色情”、“爆炸”、“恐怖”、“枪”、“军火”等，这些都不可以在网上进行传播，需要过滤掉或者用其他词语替换掉。
 */
public class WordMask {
    public static void main(String[] args) {
//        Scanner sc = new Scanner(System.in);
//        System.out.print("请发言：");
//        String input = sc.next();
        String input = "本店新到学生用品一次性饭盒、红色情书专用信封、油爆炸鸡，电影院今晚放映《恐怖游轮》、《三军火枪手》";
        System.out.println(maskWord(input));

    }

    public static String maskWord(String string) {
        StringBuilder sb = new StringBuilder(string);
        String[] dirtyWords = new String[]{
                "性", "色情", "爆炸", "恐怖", "军火", "枪"
        };

        for (String dw : dirtyWords) {
            while (sb.indexOf(dw) != -1) {
                sb.replace(sb.indexOf(dw), sb.indexOf(dw) + dw.length(), "和谐");
            }
        }

        return sb.toString();
    }
}
```
输出结果：
```text
本店新到学生用品一次和谐饭盒、红和谐书专用信封、油和谐鸡，电影院今晚放映《和谐游轮》、《三和谐和谐手》
```
# 分析一个日期是否为闰年，该月有几天，该日是星期几
```java
/*
(Scanner)当以年-月-日的格式输入一个日期时，输出其该年是否为闰年，该月有几天，该日是星期几
 */
public class DateAnalyse {
    public static void main(String[] args) throws ParseException {
        Scanner sc = new Scanner(System.in);
        System.out.print("请以「年-月-日」的格式输入一个日期：");
        String string = sc.next();

        String[] result = analyseDate(string);
        System.out.println("该日期" + result[0] + "闰年，这个月有" + result[1] + "天，这一天是" + result[2]);


    }

    public static String[] analyseDate(String string) throws ParseException {
        String[] strings = string.split("-");

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date date = sdf.parse(string);

        String[] result = new String[3];

        int year = Integer.parseInt(strings[0]);
        int month = Integer.parseInt(strings[1]);
        int day = Integer.parseInt(strings[2]);

        int[] monthDays = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        monthDays[1] = (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) ? 29 : 28;

        result[0] = (monthDays[1] == 29) ? "是" : "不是";
        result[1] = String.valueOf(monthDays[month-1]);

        SimpleDateFormat sdf2 = new SimpleDateFormat("E");
        result[2] = sdf2.format(date);

        return result;
    }
}
```
输出结果：
```text
请以「年-月-日」的格式输入一个日期：2020-1-1
该日期是闰年，这个月有31天，这一天是星期三
```
# 计算出生日期至今有多少天
```java
/*
日期练习—计算已出生多少天
 */
public class HowManyDaysSinceBirthday {
    public static void main(String[] args) throws ParseException {
        Scanner sc = new Scanner(System.in);
        System.out.print("请以「年月日」的格式输入你的生日：");
        String string = sc.next();

        System.out.println("你已出生 " + calcDays(string) + " 天");
    }

    public static long calcDays(String string) throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
        Date birthday = sdf.parse(string);
        Date today = new Date();

        long diff = today.getTime() - birthday.getTime();

        return diff / 1000 / 60 / 60 / 24;
    }
}
```
输出结果：
```text
请以「年月日」的格式输入你的生日：20190817
你已出生 137 天
```
