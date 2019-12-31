title: Java x D8
date: 2019-12-23
updated: 2019-12-23
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
IntArrayKit
构造器练习：汽车类
{% endcq %}
<!--more-->
# Homework
## 构造器练习：汽车类
创建一个汽车类，该类拥有品牌、轮胎数、车门数、颜色、重量等属性，加速、减速等行为。
并且利用构造器为每个对象设置轮胎数初始值为4，车门数为4，颜色为白色，并创建两个汽车对象，分别调用其行为;
```java
public class Car {
    public String brand;
    public int tiresCount;
    public int doorsCount;
    public String color;
    public double weight;
    public int speed;

    //加速方法
    public void accelerate(int speed) {
        this.speed += speed;
        System.out.println("Speed: " + this.speed + " km/h");
    }

    // 减速方法
    public void decelerate(int speed) {
        this.speed -= speed;
        System.out.println("Speed: " + this.speed + " km/h");
    }

    // 自定义构造器
    public Car(int tiresCount, int doorsCount, String color) {
        this.tiresCount = tiresCount;
        this.doorsCount = doorsCount;
    }

    // 无参构造器
    public Car() {
        this.tiresCount = 4;
        this.doorsCount = 4;
    }

    // 主程序入口
    public static void main(String[] args) {
        Car a = new Car(4, 4, "white");
        a.accelerate(100);

        Car b = new Car();
        b.decelerate(20);
    }
}
```

## IntArrayKit
* 创建IntArrayKit类，成员属性为int数组，
* 有一个无参构造，有一个有参构造，参数为一个int数组，并将该数组赋值给其成员变量
* 创建成员方法(行为)toString，功能为：将成员变量的数组按“[1,2,4,6]”格式进行控制台输出(可以考虑不用控制台输出，直接作为返回值返回)；
* 创建成员方法sort，功能为：将成员变量数组中数据按从小到大排序;
* 创建成员方法getMax,功能为：找出成员变量数组中最大值，并将其作为该方法的返回值返回;

```java
public class IntArrayKit {
    public int[] array;

    // Constructors
    public IntArrayKit(){}

    public IntArrayKit(int[] array){
        this.array = array;
    }

    // Member method
    public String aToString() {
        String re = "[";
        for (int i = 0; i < this.array.length; i++) {
            re += this.array[i];
            if (i != this.array.length - 1) {
                re += ",";
            }
        }
        return re + "]";
    }

    // insert sort 插入排序
    public void sort() {
        for (int i = 1; i < this.array.length; i++) {
            int tmp = this.array[i];
            int j = i - 1;
            while (j >= 0 && tmp < this.array[j]) {
                this.array[j + 1] = this.array[j];
                j--;
            }
            this.array[j + 1] = tmp;
        }
    }

    public int getMax() {
        int max = this.array[0];
        for (int a : this.array) {
            max = (max < a) ? a : max;
        }
        return max;
    }

    public static void main(String[] args) {
        int[] nums = {5,3,2,4,1};
        IntArrayKit array = new IntArrayKit(nums);
        array.sort();
        System.out.println(array.aToString());
        System.out.println(array.getMax());
    }
}
```
