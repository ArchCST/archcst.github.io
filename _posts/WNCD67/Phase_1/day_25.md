title: Java x D25
date: 2020-01-09
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
重写 equals 中 `||` 的短路用法
利用 HashSet 对字符串去重
生成10个1到20之间的不重复的随机数
奥运会分队
{% endcq %}
<!--more-->

# 重写 equals 中 `||` 的短路用法
Idea 自动生成的 override hashCode() and equals 中有这么一句，惊到了
```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    // 短路，避面了 obj == null 时调用 obj.getClass()
    if (obj == null || getClass() != obj.getClass()) return false;
    Student student = (Student) obj;
    return name.equals(student.name) &&
            id == student.id;
}
```
# 利用 HashSet 对字符串去重
```java
/*
利用 HashSet 对字符串去重
 */
public static String removeDp(String str) {
    char[] chars = str.toCharArray();
    HashSet<Character> hashSet = new HashSet<>();
    HashSet<String> s = new HashSet<>();

    StringBuilder stringBuilder = new StringBuilder();

    for (char c : chars) {
        while (!hashSet.contains(c)) {
            hashSet.add(c);
            stringBuilder.append(c);
        }
    }

    return stringBuilder.toString();
}
```
# 生成10个1到20之间的不重复的随机数
```java
/*
生成10个1到20之间的不重复的随机数
 */
public static HashSet<Integer> noRepeatRandomNumber(int count) {
    Random r = new Random();
    HashSet<Integer> hashSet = new HashSet<>();
    while (hashSet.size() < count) {
        hashSet.add(r.nextInt(20) + 1);
    }

    return hashSet;
}
```
# 奥运会分队
```java
/*
已知有十六支男子足球队参加2008 北京奥运会。写一个程序，把这16 支球队随机分为4 个组。采用List集合和随机数
2008 北京奥运会男足参赛国家：
String[] countries = {"科特迪瓦", "阿根廷", "澳大利亚", "塞尔维亚", "荷兰", "尼日利亚", "日本", "美国", "中国", "新西兰", "巴西", "比利时", "韩国", "喀麦隆", "洪都拉斯", "意大利"};
 */
public static void olympic() {
    String[] strings = {"科特迪瓦", "阿根廷", "澳大利亚", "塞尔维亚", "荷兰", "尼日利亚", "日本",
            "美国", "中国", "新西兰", "巴西", "比利时", "韩国", "喀麦隆", "洪都拉斯", "意大利"};
    ArrayList<String> arrayList = new ArrayList<>();
    Random random = new Random();

    for (String str :
            strings) {
        arrayList.add(str);
    }

    for (int i = 0; i < 4; i++) {
        StringBuilder sb = new StringBuilder();
        for (int j = 0; j < 4; j++) {
            int r = random.nextInt(arrayList.size());
            sb.append(arrayList.get(r)).append(" ");
            arrayList.remove(r);
        }
        System.out.println("Team "+i+": "+sb);
    }
}
```