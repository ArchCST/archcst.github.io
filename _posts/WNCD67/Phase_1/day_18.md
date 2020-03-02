title: Java x D18
date: 2020-01-02
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
生成四位不重复且含有至少一个大写字母一个小写字母和一个数字的验证码
{% endcq %}
<!--more-->
# 生成四位不重复且含有至少一个大写字母一个小写字母和一个数字的验证码
```java
import java.util.Random;
import java.util.Scanner;

public class ValidateCode {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (true) {
            // 生成验证码
            String vc = new ValidateCode().vcString();
            System.out.print("请输入括号中的验证码(" + vc + ")：");
            String input = sc.next();

            // 大小写不敏感
            if (input.toLowerCase().equals(vc.toLowerCase())) {
                System.out.println("验证成功！");
            } else {
                System.out.println("验证失败！");
            }
        }
    }

    public String vcString() {
        String vc = "";
        Random r = new Random();

        // 字符串中 0 代表任意，1代表数字，2代表小写字母，3代表大写字母
        StringBuilder types = new StringBuilder("0123");

        while (vc.length() < 4){
            char type = types.charAt(r.nextInt(types.length()));
            char ch = vcChar(type);
            if (!vc.contains(ch+"")) {
                vc += ch;
                types.deleteCharAt(types.indexOf(type+""));
            }
        }
        return vc;
    }

    public char vcChar(char type) {
        Random r = new Random();

        // 用参数控制生成的类型，0为参数时随机生成
        if (type == '0') {
            type = (char) (r.nextInt(3) + '1');
        }

        switch (type) {
            case '1':
                return (char) ('0' + r.nextInt(10));
            case '2':
                return (char) ('a' + r.nextInt(26));
            case '3':
                return (char) ('A' + r.nextInt(26));
        }

        return '0';
    }
}
```

# 正则邮箱判断
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/*
（需求）判断简单邮箱的验证示例：wangyijsyy@126.com
规则：邮箱首字母为字母，@符号以前字母数字下划线均可，@符号后与.符号前，字母+数字 ，然后在.后只能是3位字母。
 */
public class ValidateEmail {
    public static void main(String[] args) {
        System.out.println(validateEmail("wangyijsyy@126.com"));
        System.out.println(validateEmail("w@126.123"));
        System.out.println(validateEmail("w23_27@126sldnf.123"));
    }

    public static boolean validateEmail(String email) {
        // 规则：邮箱首字母为字母，@符号以前字母数字下划线均可，@符号后与.符号前，字母+数字 ，然后在.后只能是3位字母。
        Pattern p = Pattern.compile("^[a-z]\\w*@[a-zA-Z0-9]*\\.\\d{3}$");
        Matcher matcher = p.matcher(email);

        return matcher.matches();
    }
}
```