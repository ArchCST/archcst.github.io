title: Cheatsheet Python3 - 高级特性  
date: 2018-10-31  
updated:  
comments: true  
tags:  

-   cheatsheet

layout: post  

---

Cheatsheet 类博文详情请见 [关于页面](https://archcst.me/about/)。  
学习资料：[Python教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)  

# 切片

切片操作是取一个 list 或 tuple 部分元素的操作  

```python
classmates = ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo']

# 取 Bravo、Charlie、Delta 三个元素
print(classmates[1:4])
print(classmates[-4:-1])

# 取前 n 个元素可以省略 0
print(classmates[:3])

# 取后 n 个元素可以省略 0
print(classmates[-3:])
```

    ['Bravo', 'Charlie', 'Delta']
    ['Bravo', 'Charlie', 'Delta']
    ['Alpha', 'Bravo', 'Charlie']
    ['Charlie', 'Delta', 'Echo']

```python
L = list(range(100))

# 前十个数，隔一个取一个
print(L[:10:2])

# 所有数，隔五个取一个
print(L[::5])
```

    [0, 2, 4, 6, 8]
    [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]

也可以用于 tuple 和字符串上  

```python
# 作用于 tuple 上
t = (0,1,2,3,4,5,6,7,8,9,10)
print(t[::2])

# 作用于字符串上
s = "ABCDEFGHIJKLMN"
print(s[2:10:2])
```

    (0, 2, 4, 6, 8, 10)
    CEGI

# 封装解构

[封装解构详解](http://flowsnow.net/2016/12/23/Python%25E8%25A7%25A3%25E6%259E%2584%25E4%25B8%258E%25E5%25B0%2581%25E8%25A3%2585/)  

# 迭代

# 类

定义一个类：  

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(3, 5)
print(p.x, p.y)
```

    3 5
