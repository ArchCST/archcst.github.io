title: Java x D16
date: 2019-12-31
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
电信字符串格式化（基于 Date 类）

<span style="font-variant: small-caps;">note</span>
Date 类和 SimpleDateFormat 类的基本用法
{% endcq %}
<!--more-->
# 电信字符串格式化（基于 Date 类）
```java
import java.text.ParseException;
import java.text.SimpleDateFormat;

public class FormantCallDateWithDateClass_2 {

    public static void main(String[] args) throws ParseException {
        String string = "[2016][9][8][10][55][3]{1}[2016][9][8][10][58][57]," +
                "[2004][2][28][10][55][3]{0}[2004][3][1][10][58][57]," +
                "[2005][2][28][10][55][3]{1}[2005][3][1][10][58][57]";

        String[] data = string.split(",");

        for (String str : data) {
            System.out.println(charge(str));
        }
    }

    public static String charge(String str) throws ParseException {
        String start = str.substring(0, str.indexOf("{"));
        String end = str.substring(str.indexOf("}") + 1);
        String from = str.substring(str.indexOf('{') + 1, str.indexOf('}'));

        // core code
        SimpleDateFormat smp = new SimpleDateFormat("[yyyy][MM][dd][HH][mm][ss]");

        // calc the difference of start time and end time
        long diff = smp.parse(end).getTime() - smp.parse(start).getTime();
        diff = diff / 1000;

        // calc hours/minutes/seconds for return
        long hours = diff / 3600;
        long minutes = diff / 60 % 60;
        long seconds = diff % 60;

        // calc call charge: called 0, calling 0.3/min, ceiling the seconds.
        double money = from.equals("1") ? hours * 18 + minutes * 0.3 + ((seconds > 0) ? 0.3 : 0) : 0;

        from = from.equals("1") ? "主叫" : "被叫";

        return from + hours + "小时" + minutes + "分钟" + seconds + "秒，收费：" + money + "元";
    }
}
```
输出结果：
```text
主叫0小时3分钟54秒，收费：1.2元
被叫48小时3分钟54秒，收费：0.0元
主叫24小时3分钟54秒，收费：433.2元
```
# Date 类和 SimpleDateFormat 类的基本用法
```java
public class LearnDate {
    public static void main(String[] args) throws ParseException {
        // Date 类仍在使用的三个API：
        // 1. 无参构造，跟距当前时间生成 Date 对象
        Date d = new Date();
        System.out.println(d);

        // 2. 通过时间戳创建Date对象
        // 时间戳：距离1970年1月1日00:00:00 GMT 的毫秒数，Long类型
        Date d2 = new Date(1000L);
        System.out.println(d2);

        // 3. 获取 Date 对象的时间戳
        System.out.println(d.getTime());

        // 格式化时间输出
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(
                "yyyy年MM月dd日 HH:mm:ss E 一年中第D天 一年中第w周（有误） 本月第W周 a 时区：z");
        System.out.println(simpleDateFormat.format(d));
        String string = "2019年12月31日 16:04:17 星期二 一年中第365天 一年中第1周（有误） 本月第5周 下午 时区：CST";

        // 把字符串通过 SimpleDateFormat 转换回 Date 对象
        Date d3 = simpleDateFormat.parse(string);
        System.out.println(d3);


        // 早于1970年的时间的时间戳是负吗？是的
        SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
        Date d4 = sdf.parse("16540302");
        System.out.println(d4.getTime());
    }
}
```

输出结果：
```text
Tue Dec 31 17:39:37 CST 2019
Thu Jan 01 08:00:01 CST 1970
1577785177095
2019年12月31日 17:39:37 星期二 一年中第365天 一年中第1周（有误） 本月第5周 下午 时区：CST
Tue Dec 31 16:04:17 CST 2019
-9966787200000
```
