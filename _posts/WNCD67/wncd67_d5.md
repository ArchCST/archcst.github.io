title: Java x D5
date: 2019-12-20
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
简单电话号码判断
拼接数组
冒泡排序
选择排序
{% endcq %}
<!--more-->

# 简单电话号码判断
```java
public static boolean isTel(String str) {
    int i;
    for (i = 0; i < str.length(); i++) {
        if (str.charAt(i) < 47 || str.charAt(i) > 58) {
            break;
        }
    }

    /* 精华！*/
    return str.charAt(0) == '1' && i == 11;
}
```

# 拼接数组
定义两个 int 型数组 firstBuf 的 secondBuf，将其连接为一个大数组 buf。
对 buf 数组进行升序排序，要求不使用自带的 sort 方法
```java
public static void forEach() {
    int[] firstBuf = {4, 7, 3, 1, 6};
    int[] secondBuf = {5, 8, 2, 4, 9};
    int[] buf = new int[(firstBuf.length + secondBuf.length)];

    for (int i = 0; i < buf.length; i++) {
        if (i < firstBuf.length) {
            buf[i] = firstBuf[i];
        } else {
            buf[i] = secondBuf[i - firstBuf.length];
        }
    }

    System.out.println(Arrays.toString(bubbleSort(buf)));
    System.out.println(Arrays.toString(selectSort(buf)));
}
```

# 冒泡排序
```java
public static int[] bubbleSort(int[] arr) {
    int times = 0;
    for (int i = arr.length - 1; i > 0; i--) {
        boolean isSwapped = false;
        for (int j = 0; j < i - 1; j++) {
            times++;
            if (arr[j] > arr[j + 1]) {
                isSwapped = true;
                arr[j] ^= arr[j + 1];
                arr[j + 1] ^= arr[j];
                arr[j] ^= arr[j + 1];
            }
        }
        if (!isSwapped) {
            break;
        }
    }
    System.out.println("bubbleSort times: " + times);
    return arr;
}
```

# 选择排序
```java
public static int[] selectSort(int[] arr) {
    int maxIndex;
    int times = 0;
    for (int i = arr.length - 1; i > 0; i--) {
        maxIndex = 0;
        int j;
        for (j = 0; j <= i; j++) {
            times++;
            maxIndex = (arr[j] > arr[maxIndex]) ? j : maxIndex;
        }
        if (i != maxIndex) {
            arr[i] ^= arr[maxIndex];
            arr[maxIndex] ^= arr[i];
            arr[i] ^= arr[maxIndex];
        }
    }
    System.out.println("selectSort times: " + times);
    return arr;
}
```
