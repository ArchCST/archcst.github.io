title: Java x D12
date: 2019-12-27
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
动态绑定的坑
多态：宠物喂养
{% endcq %}
<!--more-->
# 动态绑定的坑
Parent child = new Child(); 中：
编译类型是 Parent，运行时类型是 Child
编译时看编译类型，也就是声明类型
调用对象的成员变量 `child.foo` 时，如果父子类存在冲突，以编译类型为准
调用对象的成员方法 `child.foo()` 时，如果父子类存在冲突，以运行时类型为准
# 多态：宠物喂养
用多态的概念，实现多个宠物类的封装：
{% tabs petfeed %}
<!-- tab class Animal -->
```java
public abstract class Animal {
    protected String name;
    protected int happyValue;
    protected int energy;

    protected abstract void call();

    public abstract void eat();
    
    public void reportStatus() {
        System.out.println(name+" happy:"+happyValue+" energy:"+energy);
    }

    public Animal(String name, int happyValue, int energy) {
        this.name = name;
        this.happyValue = happyValue;
        this.energy = energy;
    }

    public int getHappyValue() {
        return happyValue;
    }

    public void setHappyValue(int happyValue) {
        this.happyValue = happyValue;
    }

    public int getEnergy() {
        return energy;
    }

    public void setEnergy(int energy) {
        this.energy = energy;
    }
}
```
<!-- endtab -->

<!-- tab class Cat -->
```java
public class Cat extends Animal{
    private int earCount;

    public Cat(String name, int happyValue, int energy) {
        super(name, happyValue, energy);
        earCount = 2;
    }

    public void call() {
        System.out.println("喵喵喵");
    }

    public void eat() {
        this.happyValue += 20;
        this.energy += 50;
        call();
    }
}
```
<!-- endtab -->

<!-- tab class Fish -->
```java
public class Fish extends Animal {
    private int fins;

    public Fish(String name, int happyValue, int energy) {
        super(name, happyValue, energy);
        fins = 3;
    }

    @Override
    public void call() {
        System.out.println("。。。");
    }

    public void eat() {
        happyValue += 5;
        energy += 100;
        call();
    }

    public void swim(){
        System.out.println("Fish swimming");
    }
}
```
<!-- endtab -->

<!-- tab class Person -->
```java
public class Person {
    private String name;
    private int happyValue;
    private int energy;
    Animal[] animals;

    public void setAnimals(Animal... animals) {
        this.animals = animals;
    }


    public Person(String name, int happyValue, int energy) {
        this.name = name;
        this.happyValue = happyValue;
        this.energy = energy;
    }

    public String getName() {
        return name;
    }

    public void feed(Animal animal){
        System.out.println(name+"喂了"+animal.name);
        happyValue += 10;
        energy -= 5;
        animal.eat();
    }

    public void feedAll(){
        System.out.println(name+"把所有宠物都喂了");
        for (Animal animal : animals) {
            feed(animal);
        }
    }

    public void petsStatus(){
        for (Animal animal : animals) {
            animal.reportStatus();
        }
    }
    public void reportStatus(){
        System.out.println(name+" happy:"+happyValue+" energy:"+energy);
    }
}
```
<!-- endtab -->

<!-- tab class DemoMain -->
```java
import java.util.Scanner;

public class DemoMain {
    public static void main(String[] args) {
        Cat cat = new Cat("小喵喵", 30, 30);
        Fish fish = new Fish("小鱼鱼", 5, 5);
        Person person = new Person("敏姐", 5, 100);

        person.setAnimals(cat, fish);

        Scanner sc = new Scanner(System.in);
        char input;

        do {
            System.out.println("1. 喂猫   2. 喂鱼   3. 都喂   4. 查看状态   0. 退出");
            input = sc.next().charAt(0);
            switch (input) {
                case '1':
                    person.feed(person.animals[0]);
                    break;
                case '2':
                    person.feed(person.animals[1]);
                    break;
                case '3':
                    person.feedAll();
                    break;
                case '4':
                    person.reportStatus();
                    person.petsStatus();
                    break;
                case '0':
                    System.out.println("撒哟啦啦");
                    break;
                default:
                    System.out.println("输入错误");
                    break;
            }

        } while (input != '0');
    }
}
```
<!-- endtab -->
{% endtabs %}