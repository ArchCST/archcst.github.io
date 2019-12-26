title: Java x D10
date: 2019-12-25
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">note</span>
*Java 中父类和子类的运行顺序*

<span style="font-variant: small-caps;">homework</span>
{% endcq %}
<!--more-->

# Java 中父类和子类的运行顺序
定义两个类，Parent 的 Child，分别定义普通成员变量和类变量。

然后分别在两个类的静态代码块 `static{}` 、构造代码块`{}` 和 `构造器` 中判断这四个变量是否为 `1` 并输出结果，大致代码结构如下：


{% tabs structure %}
<!-- tab 父类 Parent -->
```java
public class Parent {
    public static int parentStaticInt = 1;
    public int parentInt = 1;

    static {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }

    {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }

    public Parent() {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }
}
```
<!-- endtab -->

<!-- tab 子类 Child -->
```java
public class Child extends Parent {
    public static int childStaticInt = 1;
    public int childInt = 1;

    static {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }

    {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }

    public Child() {
        对四个变量分别判断是否为1：
            如果报错则说明变量还未声明未开辟空间
            如果为1说明已赋值，不为1说明未赋值
    }

    public static void main(String[] args) {
        System.out.println("        !!! Child's main !!!");
        new Child();
    }
}
```
<!-- endtab -->

<!-- tab 第三个类 ThirdClass -->
```java
public class ThirdClass {
    static {
        System.out.println("      ThirdClass's  静态代码块");
    }
    {
        System.out.println("      ThirdClass's  构造代码块");
    }
    public ThirdClass() {
        System.out.println("        ThirdClass's  构造器");
    }
    public static void main(String[] args) {
        System.out.println("     !!! ThirdClass's  main !!!");
        new Child();
        new ThirdClass();
    }
}
```
<!-- endtab -->
{% endtabs %}

完整代码在这里，有点长，点开查看：
{% tabs code, 4 %}
<!-- tab 父类 Parent -->
```java
public class Parent {
    public static int parentStaticInt = 1;
    public int parentInt = 1;

    static {
        System.out.println("-----------父类静态代码块-----------");
        // childStaticInt
        if (Child.childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        /* childInt 报错：静态资源无法访问非静态资源
        if (Child.childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
         */
        System.out.println("| childInt            报错：未声明 |");
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        /* parentInt 报错：静态资源无法访问非静态资源
        if (parentInt == 1) {
            System.out.println("| parentInt          已声明，已赋值 |");
        } else {
            System.out.println("| parentInt          已声明，未赋值 |");
        };
         */
        System.out.println("| parentInt           报错：未声明 |"); // if (parentInt != 1) {} 报错
        System.out.println("------------------------------------");
        System.out.println();
    }

    {
        System.out.println("-----------父类构造代码块-----------");
        // childStaticInt
        if (Child.childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        /* childInt 报错：静态资源无法访问非静态资源
        if (Child.childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
         */
        System.out.println("| childInt            报错：未声明 |");
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        // parentInt
        if (parentInt == 1) {
            System.out.println("| parentInt         已声明，已赋值 |");
        } else {
            System.out.println("| parentInt         已声明，未赋值 |");
        };
        System.out.println("------------------------------------");
        System.out.println();
    }

    public Parent() {
        System.out.println("-------------父类构造器-------------");
        // childStaticInt
        if (Child.childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        /* childInt 报错：静态资源无法访问非静态资源
        if (Child.childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
         */
        System.out.println("| childInt            报错：未声明 |");
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        // parentInt
        if (parentInt == 1) {
            System.out.println("| parentInt         已声明，已赋值 |");
        } else {
            System.out.println("| parentInt         已声明，未赋值 |");
        };
        System.out.println("------------------------------------");
        System.out.println();
    }
}
```
<!-- endtab -->

<!-- tab 子类 Child -->
```java
public class Child extends Parent {
    public static int childStaticInt = 1;
    public int childInt = 1;

    static {
        System.out.println("-----------子类静态代码块-----------");
        // childStaticInt
        if (childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        /* childInt 报错：静态资源无法访问非静态资源
        if (childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
        */
        System.out.println("| childInt            报错：未声明 |");
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        /* parentInt 报错：静态资源无法访问非静态资源
        if (parentInt == 1) {
            System.out.println("| parentInt          已声明，已赋值 |");
        } else {
            System.out.println("| parentInt          已声明，未赋值 |");
        };
        */
        System.out.println("| parentInt           报错：未声明 |"); // if (parentInt != 1) {} 报错
        System.out.println("------------------------------------");
        System.out.println();
    }

    {
        System.out.println("-----------子类构造代码块-----------");
        // childStaticInt
        if (childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        // childInt
        if (childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        // parentInt
        if (parentInt == 1) {
            System.out.println("| parentInt         已声明，已赋值 |");
        } else {
            System.out.println("| parentInt         已声明，未赋值 |");
        };
        System.out.println("------------------------------------");
        System.out.println();
    }

    public Child() {
        System.out.println("-------------子类构造器-------------");
        // childStaticInt
        if (Child.childStaticInt == 1) {
            System.out.println("| childStaticInt    已声明，已赋值 |");
        } else {
            System.out.println("| childStaticInt    已声明，未赋值 |");
        };
        // childInt
        if (childInt == 1) {
            System.out.println("| childInt          已声明，已赋值 |");
        } else {
            System.out.println("| childInt          已声明，未赋值 |");
        };
        // parentStaticInt
        if (parentStaticInt == 1) {
            System.out.println("| parentStaticInt   已声明，已赋值 |");
        } else {
            System.out.println("| parentStaticInt   已声明，未赋值 |");
        }
        // parentInt
        if (parentInt == 1) {
            System.out.println("| parentInt         已声明，已赋值 |");
        } else {
            System.out.println("| parentInt         已声明，未赋值 |");
        };
        System.out.println("------------------------------------");
        System.out.println();
    }

    public static void main(String[] args) {
        System.out.println("        !!! Child's main !!!");
        new Child();
    }
}
```
<!-- endtab -->

<!-- tab 第三个类 ThirdClass -->
```java
public class ThirdClass {
    static {
        System.out.println("      ThirdClass's  静态代码块");
    }
    {
        System.out.println("      ThirdClass's  构造代码块");
    }
    public ThirdClass() {
        System.out.println("        ThirdClass's  构造器");
    }
    public static void main(String[] args) {
        System.out.println("     !!! ThirdClass's  main !!!");
        new Child();
        new ThirdClass();
    }
}
```
<!-- endtab -->

<!-- tab 收起来 -->
点其它 tab 展开
<!-- endtab -->
{% endtabs %}
输出的结果是：
{% tabs result %}
<!-- tab 运行 Child -->
```text
-----------父类静态代码块-----------
| childStaticInt    已声明，未赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt           报错：未声明 |
------------------------------------

-----------子类静态代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt           报错：未声明 |
------------------------------------

        !!! Child's main !!!
-----------父类构造代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-------------父类构造器-------------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-----------子类构造代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt          已声明，已赋值 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-------------子类构造器-------------
| childStaticInt    已声明，已赋值 |
| childInt          已声明，已赋值 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------
```
<!-- endtab -->

<!-- tab 运行 ThirdClass -->
```text
      ThirdClass's  静态代码块
     !!! ThirdClass's  main !!!
-----------父类静态代码块-----------
| childStaticInt    已声明，未赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt           报错：未声明 |
------------------------------------

-----------子类静态代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt           报错：未声明 |
------------------------------------

-----------父类构造代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-------------父类构造器-------------
| childStaticInt    已声明，已赋值 |
| childInt            报错：未声明 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-----------子类构造代码块-----------
| childStaticInt    已声明，已赋值 |
| childInt          已声明，已赋值 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

-------------子类构造器-------------
| childStaticInt    已声明，已赋值 |
| childInt          已声明，已赋值 |
| parentStaticInt   已声明，已赋值 |
| parentInt         已声明，已赋值 |
------------------------------------

      ThirdClass's  构造代码块
        ThirdClass's  构造器
```

<!-- endtab -->
{% endtabs %}
分析一下 Child.java 的运行结果，可以得知 Java 的运行机制是这样的：
1. 从 `Child's main` 不在第1行可知：Java 找到 main() 方法后并不直接执行 main() 里面的代码，而是首先把 main() 所在的类加载到方法区。
2. 第2行和第4行，`childStaticInt` 和 `parentStaticInt` 未报错说明在类加载到方法区时静态变量就已完成了声明（空间开辟）。
3. 第2行，`childStaticInt` 未赋值说明变量的声明操作和赋值操作是分开的，即使写在一行声明时即赋值。
4. 第1行，父类和子类的空间都开辟完成后，立即开始执行父类静态代码块内容。
5. 第4行 `parentStaticInt` 已有值，说明当前类的静态变量赋值操作先于静态代码块的执行。
6. 第8行，开始子类静态代码块执行，说明在父类的静态代码块执行完成后，父类就已经完成了方法区的全部加载过程。
7. 第9行，和第4行一样，在子类静态代码块执行前先对静态变量 `childStaticInt` 赋值。
8. 第15行，main() 方法所在类加载到方法区完成后，开始执行 main() 方法，遇到 new Child() 后开始实例化 Child 类。
9. 第16行，实例化子类时会优先对父类进行构造，构造代码块会在构造器之前执行。
10. 第18行 `childInt` 报错说明实例化时是父类优先。
11. 第20行 `parentInt` 已有值说明在构造代码块执行前，普通成员变量就已经完成了空间开辟和赋值。
12. 第23、30、37行说明在父类构造代码块完成后，接下来依次执行了父类构造器、子类构造代码块、子类构造器
13. 第32行最终在子类构造块执行前对 `childInt` 进行了赋值。

另外，从运行 ThirdClass.java 的结果来看，Java 并不会因为执行了 main() 方法就对 main() 方法所在的类进行实例化（构造），但只要加载类到方法区，就会执行静态代码块以及对静态变量赋值。

所以，Java对继承类的加载顺序是这样的：
1. 父类加载到方法区，完成父类静态变量声明（空间开辟）
2. 子类加载到方法区，完成子类静态变量声明（空间开辟）
3. 父类静态变量赋值
4. 父类静态代码块执行
5. 子类静态变量赋值
6. 子类静态代码块执行
7. mian() 方法，遇到创建对象时开始实例化对象
7. 父类成员变量声明、赋值
8. 父类构造代码块执行
9. 父类构造器执行
10. 子类成员变量声明、赋值
11. 子类构造代码块执行
12. 子类构造器执行


