title: Java x D20
date: 2020-01-04
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
equals 方法和 == 的区别
File 类的用法
递归搜索本地特定后缀的文件
{% endcq %}
<!--more-->
# equals 方法和 == 的区别
`equals()` 方法本质上和 `==` 是没有区别的，因为该方法来自于Object类，底层实现都是 `==`。
但是 `equals()` 是可以通过重写来赋与不同的意义。
`==` 是对栈内的内容进行比较，无论其是存的指向对象的地址还是存的基本数据类型。
# File 类的用法
```java
import java.io.File;
import java.io.IOException;

/*
Java 以对象的形式管理文件
 */
public class LearnFile {
    public static void main(String[] args) {
        //  创建文件对象的几个构造器
        File f1 = new File("/cst/git/WNJava/ioFiles/"); // Linux 非「/」开头的会自动识别为相对路径
        File f2 = new File("/git/","/WNJava/ioFiles/");
        File f3 = new File(f1, "f1.docx");

        // Linux/Windows 下获取 home 的位置
        File home = new File(System.getProperty("user.home"));

        // 创建新目录：
        File ioFiles = new File(home,"/git/WNJava/LearnJava/ioFiles");
        System.out.println("创建目录是否成功："+ioFiles.mkdir());

        // 递归创建目录：mkdirs()
        File lotOfDirs = new File(ioFiles, "/a/b/c/d");
        System.out.println("递归创建目录是否成功："+lotOfDirs.mkdirs());


        // 创建新文件：
        File newMDFile = new File(ioFiles, "f1.md");
        try {
            System.out.println("文件创建是否成功："+newMDFile.createNewFile());
        } catch (IOException e) { // 一般是磁盘拒绝访问，断开连接
            e.printStackTrace();
        }

        // File 的基本 APIs：
        File file = new File(home, "/git/WNJava/LearnJava/ioFiles/f1.md");
        fileAPIs(file);

        // 获取目录内所有文件和目录**对象**，注意空指针异常
        File[] filesIn_ioFiles = ioFiles.listFiles();
        for (File f : filesIn_ioFiles){
            System.out.println(ioFiles.getAbsolutePath()+"下的所有文件的相对路径："+f.getAbsolutePath());
            // 删除操作：直接删，不进 Trash，危险
            f.delete();
        }

        // 获取目录内所有文件名和目录**名**，注意空指针异常
        String[] fileNames = ioFiles.list();
        for (String str : fileNames) {
            System.out.println(ioFiles.getAbsolutePath()+"下有："+str);
        }


    }
    public static void fileAPIs(File file) {
        System.out.println("文件名："+file.getName());
        System.out.println("文件的相对路径："+file.getPath());
        System.out.println("文件的绝对路径："+file.getAbsolutePath());
        System.out.println("文件的父类路径："+file.getParent());  // 通过相对路径构建的文件的父路径为 null
        System.out.println("文件是否存在："+file.exists());
        System.out.println("文件是否可读："+file.canRead());
        System.out.println("文件是否可写："+file.canWrite());
        System.out.println("文件是否是一个文件："+file.isFile());
        System.out.println("文件是否是一个目录："+file.isDirectory());
        System.out.println("文件最后被修改的时间："+file.lastModified());
        System.out.println("文件的长度："+file.length());
    }
}
```
# 递归搜索本地特定后缀的文件
```java
import java.io.File;
import java.util.Scanner;

public class CustomListFiles {
    public int count = 0;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入搜索目录：");
        String dir = sc.next();
        System.out.println("请输入文件后缀，多后缀用英文逗号分割：");
        String extInput = sc.next();

        String[] exts = extInput.split(",");

        // 为后缀添加「.」
        for (String ext:exts) {
            if (!ext.startsWith(".")) {
                ext = "." + ext;
            }
        }

        CustomListFiles cf = new CustomListFiles();
        File file = new File(dir);
        if (!file.isDirectory()) {
            System.out.println(file.getAbsolutePath()+" 不是一个目录");
        } else {
            cf.fileSearch(file, exts);
        }

        System.out.println("匹配的文件共： "+cf.count+" 个");
    }

    public void fileSearch(File file, String[] exts) {
        File[] files = file.listFiles();

        for (File f:files) {
            if (f.isDirectory()) {
                // 递归
                fileSearch(f, exts);
            } else if (f.isFile()) {
                String path = f.getAbsolutePath();
                for (String e : exts) {
                    if (path.endsWith(e)) {
                        System.out.println(path);
                        count++;
                    }
                }
            }
        }
    }
}
```
