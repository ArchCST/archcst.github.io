title: Java x D6
date: 2019-12-21
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">weektest 1</span>
计算某日期是一年中的第几天
给n个数字，打印互不相同且无重复数字的三位数？
{% endcq %}
<!--more-->
# 计算某日期是一年中的第几天
```java
public int nthOfYear(int date) {
    int[] days = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    int result = 0;

    // 分解日期
    int y = date / 10000;
    int m = date % 10000 / 100;
    int d = date % 100;

    // 闰年判断
    days[1] = (y % 4 == 0 && y % 100 != 0 || y % 400 == 0) ? 29 : 28;

    // 计算月份所含天数
    for (int i = 1; i < m; i++) {
        result += days[i - 1];
    }

    // 加上天数
    return result += d;
}
```

# 给n个数字，打印互不相同且无重复数字的三位数？
```java
public void assembleNumber(int[] nums) {
    for (int a = 0; a < nums.length; a++) {
        for (int b = 0; b < nums.length; b++) {
            if (b == a) continue;
            for (int c = 0; c < nums.length; c++) {
                if (c != a && c != b) {
                    System.out.println("" + nums[a] + nums[b] + nums[c]);
                }
            }
        }
    }
}
```
