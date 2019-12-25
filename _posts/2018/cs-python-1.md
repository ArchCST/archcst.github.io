title: Cheatsheet Python3 - 基础  
date: 2018-09-28  
updated: 2018-11-22  
comments: true  
tags:  

-   cheatsheet

layout: post  

---

Cheatsheet 类博文详情请见 [关于页面](https://archcst.me/about/)。  
为便于理解，本文注释使用了中文，遵循 [PEP 8](https://www.python.org/dev/peps/pep-0008/) 不应该在代码中使用中文。  
学习资料：[Python教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)  

# Python 基础

使用 `~#~` 作为注释  
以 `~:~` 作为结尾的语句，下接缩进的 `语句块` 。  
约定俗成，避免错误，坚持使用 `4个空格` 的缩进。  
Python 大小写敏感。  

<!--more-->

使用 help() 查询帮助，括号中可以为变量、对象、类名、函数名、方法名。  

```python
# 查询 list.append 函数
help(list.append)
```

    Help on method_descriptor:
    
    append(...)
        L.append(object) -> None -- append object to end

{% note info %}  
其中 `->` 表示返回值，意味执行 append 函数后返回值为 None  
{% endnote %}  

# 数据类型和变量

## 各进制整数

十进制   ：  `1` 或 `0d1`  
二进制   ：  `0b1`  
八进制   ：  `0o1`  

<!-- more -->

十六进制 ：  `0x1`  

```python
print(20, 0b10100, 0o24, 0x14)
```

    20 20 20 20

另外 Python 3 中已经取消了长整数，所以 int 的长度理论上是没有限制的。  

## 科学计数法

```python
# 浮点数
print(1.23, -9.00, sep=" ")

# 科学计数法
print(123e3, 1.23e5, 12.3e4, sep=" ")
```

    1.23 -9.0
    123000.0 123000.0 123000.0

## 字符串

字符串是不可变对象（和 tuple 类似）。  

```python
# 使用单引号或双引号括起来
print('abc', "abc", 'a"b"c',"a'b'c", sep=" ")

# 使用 \ 转义
print('a\'b\'c', "a\"b\"c", "a\\b\\c", sep=" ")

```

    abc abc a"b"c a'b'c
    a'b'c a"b"c a\b\c

```python
# 使用 '''...''' 或 """...""" 输入多行内容
print('''Line 1
Line 2
Line 3''', 
"""Line 3.1
Line 5
Line 6""")

# 也可以避免对单引号和双引号的混淆，在 SQL 语句中特别好用
sql = """SELECT * FROM user WHERE name='tom'"""
print(sql)
```

    Line 1
    Line 2
    Line 3 Line 3.1
    Line 5
    Line 6
    SELECT * FROM user WHERE name='tom'

```python
# 在字符串前使用 r 表示自然语句使用
print(r"\n \t")

# 在处理 Windows 目录的时候特别好用
print("Without 'r': ", "C:\notebook\todo")
print("With 'r': ", r"C:\notebook\todo")
```

    \n \t
    Without 'r':  C:
    otebook	odo
    With 'r':  C:\notebook\todo

## 布尔值

```python
print(True, False, 3>2, 2>3, sep=" ")
print(True and True, True and False, False and False, sep=" ")
print(True or True, True or False, False or False, sep=" ")
print(5>3 and 3>1, 5>3 or 1>3, sep=" ")
print(not True, not False, not 5>3)
```

    True False True False
    True False False
    True True False
    True True
    False True False

## 空值

```python
# Python 中空值为 None，空值在任何语言中都不“等于”0
print(None, 0 == None)

```

    None False

## 变量

```python
# 合法变量名由[a-z]、[A-Z]、[0-9]、[_]组成，不能由字母开头
n = 1
b = False
s = 'string'
print(n, b, s, sep=" ")

# Python 是动态语言
n = 2.0
b = 'String'
s = True
print(n, b, s, sep=" ")

# 可以把值赋予另一个变量
f = b
b = 3
print(f, b)
```

    1 False string
    2.0 String True
    String 3

## 常量

```python
# Python 中通常用全大写表示“变量”，但其实它依旧是变量。
PI = 3.14159265359
print(PI)
PI = "So you changed it"
print(PI)
```

    3.14159265359
    So you changed it

# 字符编码

[字符串和编码 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431664106267f12e9bef7ee14cf6a8776a479bdec9b9000)  

对于单个字符的处理，用 `ord()` 实现字符转编码，用 `chr()` 实现编码转字符：  

```python
print(r"ord('B') =", ord('B'))
print(r"chr(66) =", chr(66))
print(r"ord('中') =", ord("中"))
print(r"chr(25991) =", chr(25991))
print('\u4e2d\u6587')
```

    ord('B') = 66
    chr(66) = B
    ord('中') = 20013
    chr(25991) = 文
    中文

一般来说，在源代码中包含中文的时候，保存源码时需要指定保存为 UTF-8 编码。为了让解释器读取源码时按 UTF-8 读取，通常在文件开头写：  

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

# 格式化字符串

## C 方式

python2 和 python3 都支持 C 方式的占位符 `%`，当字符串中的占位符只有一个的时候，括号可以省略，形式如下：  

```python
print("Hello, %s" % "world")
print("Life is %s, you need %s" % ("short", "python"))
```

    Hello, world
    Life is short, you need python

{% note info %}  
`%` 后面只能有一个对象，所以多个内容的时候用一个 tuple 作为一个对象。  
{% endnote %}  

占位符有以下几种：  

| 占位符 | 替换内容 |
|------ |------ |
| `~%d~` | 整数   |
| `~%f~` | 浮点数 |
| `~%s~` | 字符串 |
| `~%x~` | 十六进制整数 |

其中 `%s` 万用，它会把任何数据类型转换为字符串。  

整数与小数的位数，补 0：  

```python
print('%2d - %02d' % (3, 1))
print('%.2f' % 3.1415926)
```

     3 - 01
    3.14

当需要转义 `%` 时，使用 `%%` 。  

## Python 方式

调用 `strings` 对象的 `formant()` 函数是更 pythonic 的方法。  

```python
# 占位符 {}
print("Life is {}, you need {}".format("short", "python"))

# 位置参数
print("Time is {2}".format("time","gold","money"))

# 关键字参数或命名参数。位置参数按序号匹配，关键字参数按名词匹配
print('{server} {1}:{0}'.format(8888,'192.168.1.100', server="Web Server Info:"))

# 访问元素
print('{0[0]}  {1[1]}'.format('Alpha', 'Bravo'))
```

    Life is short, you need python
    Time is money
    Web Server Info: 192.168.1.100:8888
    A  r

### 对齐，保留位数等

`:` 前为未知参数或命名关键字参数，`:` 后为格式  

```python
# 两位小数
print("{:.2f}".format(6))

# 占10位，右对齐
print("|{: >10}|".format("ABC"))

# 占10位，左对齐
print("|{: <10}|".format("ABC"))

# 转义 {}
print("{{}} {}".format(5))

# 居中
print('{:^30}'.format('centered'))

# 居中填充字符
print('{:-^30}'.format('centered'))
```

    6.00
    |       ABC|
    |ABC       |
    {} 5
               centered           
    -----------centered-----------

### 进制

```python
print("int: {0:d}; hex: {0:x} or {0:X}; oct: {0:o}; bin: {0:b}".format(100))
print("int: {0:#d}; hex: {0:#x} or {0:#X}; oct: {0:#o}; bin: {0:#b}".format(100))
```

    int: 100; hex: 64 or 64; oct: 144; bin: 1100100
    int: 100; hex: 0x64 or 0X64; oct: 0o144; bin: 0b1100100

# 字符串操作

## join

```python
# 使用 + 返回新的字符串
str0 = 'Hello'
str1 = 'World!'
print(str0 + " " + str1)

# 使用 join 方法 'string'.join(iterable) -> str
print('!'.join(str0))
```

    Hello World!
    H!e!l!l!o

## split

split(sep=None, maxsplit = -1) -> list of strings  
从左至右分割字符串，去掉分隔符 `sep`  
`sep` 指定分割串，默认为空白字符串  
`maxsplit` 指定分割的次数， `-1` 表示遍历整个字符串  

```python
str = "I'm \ta super student."
print(str.split())
print(str.split('s'))
print(str.split('super'))
print(str.split('super '))
print(str.split(' '))
print(str.split(' ', maxsplit=2))
print(str.split('\t', maxsplit=2))
```

    ["I'm", 'a', 'super', 'student.']
    ["I'm \ta ", 'uper ', 'tudent.']
    ["I'm \ta ", ' student.']
    ["I'm \ta ", 'student.']
    ["I'm", '\ta', 'super', 'student.']
    ["I'm", '\ta', 'super student.']
    ["I'm ", 'a super student.']

`rsplit` 函数可以反向的切，使用方式一样，主要区别在限定 `maxsplit` 时：  

```python
# 和上面一样的代码，区别在倒数第二行
str = "I'm \ta super student."
print(str.rsplit())
print(str.rsplit('s'))
print(str.rsplit('super'))
print(str.rsplit('super '))
print(str.rsplit(' '))
print(str.rsplit(' ', maxsplit=2))
print(str.rsplit('\t', maxsplit=2))
```

    ["I'm", 'a', 'super', 'student.']
    ["I'm \ta ", 'uper ', 'tudent.']
    ["I'm \ta ", ' student.']
    ["I'm \ta ", 'student.']
    ["I'm", '\ta', 'super', 'student.']
    ["I'm \ta", 'super', 'student.']
    ["I'm ", 'a super student.']

splitlines([keepends]) -> list of strings  
作用于 `\n`、`\r` 和 `\r\n`  
`keepends` 为布尔，表示是否保留换行符，默认不保留  

```python
str = 'AB C\nDE FG\rHIJK\r\n'
print(str)
print(str.splitlines())
print(str.splitlines(True))
```

    AB C
    DE FG
HIJK
    
    ['AB C', 'DE FG', 'HIJK']
    ['AB C\n', 'DE FG\r', 'HIJK\r\n']

## partition

partition(sep) -> (head, sep, tail)  
从左至右找第一个 `sep`，返回一个三元组（tuple）包含分隔符前、分隔符、分隔符后三段内容。  
如果没有找到，则返回整个字符串为 `head`， `sep` 和 `tail` 为空的三元组。  

```python
str = "I'm \ta super student."
print(str.partition('\t'))
print(str.partition('f'))
```

    ("I'm ", '\t', 'a super student.')
    ("I'm \ta super student.", '', '')

和 split 类似，也有 `rpartition` 函数：  

```python
str = "I'm \ta super student."
print(str.rpartition('\t'))
print(str.rpartition('f'))
```

    ("I'm ", '\t', 'a super student.')
    ('', '', "I'm \ta super student.")

## 大小写转换

```python
str = 'Alpha killed Bravo with Charlie'
print(str.upper())
print(str.lower())
print(str.swapcase())
```

    ALPHA KILLED BRAVO WITH CHARLIE
    alpha killed bravo with charlie
    aLPHA KILLED bRAVO WITH cHARLIE

## 字符串排版及对齐

```python
print('str.title string. capitalize every words'.title())
print('str.capitalize string. only capitalize first word'.capitalize())
print('str.center string'.center(50, '='))
print('12.00 dollar, fill left with 0'.zfill(50))
print('str.ljust string'.ljust(50, '-'))
print('str.rjust string'.rjust(50, '+'))
```

    Str.Title String. Capitalize Every Words
    Str.capitalize string. only capitalize first word
    ================str.center string=================
    0000000000000000000012.00 dollar, fill left with 0
    str.ljust string----------------------------------
    ++++++++++++++++++++++++++++++++++str.rjust string

## replace

字符串是不可变对象，所以修改是返回新的字符串。  

replace(old, new[, count]) -> str  

```python
str = 'this is a string with lots of "s"'

print(str.replace('this', 'that'))
print(str.replace('s', '?'))
print(str.replace('s', '?', 2))
```

    that is a string with lots of "s"
    thi? i? a ?tring with lot? of "?"
    thi? i? a string with lots of "s"

## strip

从两端去除字符串，默认去除空白字符和换行符。  
strip([chars]) -> str  

```python
str = '\n\t      content   \n'
print(str)
print('After strip:')
print(str.strip())
```

    
    	      content   
    
    After strip:
    content

```python
# 有 [chars] 的时候从两边去除指定的 chars，可以指定多个 chars
str = '   abcd abcd   '
print(str.strip('a'))
print(str.strip('a '))
print(str.strip('a d'))
print(str.strip('bcd'))
print(str.strip(' bcd'))
```

       abcd abcd   
    bcd abcd
    bcd abc
       abcd abcd   
    abcd a

可以用 `lstrip` 和 `rstrip` 指定去除哪边的字符  

```python
str = '  abcd dcba  '
print(str.lstrip(' '))
print(str.rstrip(' '))
print(str.lstrip('ab '))
print(str.rstrip('ab '))
```

    abcd dcba  
      abcd dcba
    cd dcba  
      abcd dc

## find index

find(sub[, start[, end]]) -> int  
从 `start` 到 `end` 的区间内从左至右查找子字符串，返回索引，未找到则返回 `-1`  

rfind 是从右至左查找  

```python
str = 'Alpha Bravo Bravo Charlie Bravo'
print(str.find('Bravo'))
print(str.rfind('Bravo'))
print(str.find('Bravo', 8))
print(str.find('Bravo', 8, 13))
print(str.find('Bravo', 5, -1))

# 区间左包右不包，因为 -1 不包含 o，所以找不到：
print(str.find('Bravo', -10, -1))
```

    6
    26
    12
    -1
    6
    -1

`index` 的用法和 `find` 几乎一致，但 `index` 在未找到的时候会抛出异常 `ValueError` 。  

```python
str = 'Alpha Bravo Bravo Charlie Bravo'
print(str.index('Bravo'))
print(str.rindex('Bravo'))
print(str.index('Bravo', 8))
print(str.index('Bravo', 5, -1))
```

    6
    26
    12
    6

## count

count(sub[, start[, end]]) -> int  
从 `start` 到 `end` 的区间内从左至右统计子字符串出现次数  

```python
str = 'Alpha Bravo Bravo Charlie Bravo'

print(str.count('Bravo'))
print(str.count('Bravo', 7))
print(str.count('Bravo', 7, 8))
```

    3
    2
    0

## endswith startswith

startswith(prefix[, start[, end]]) -> bool  
判断字符串的开头  

endswith(suffix[, start[, end]]) -> bool  
判断字符串的结尾  

```python
str = 'Alpha Bravo Bravo Charlie Bravo'

print(str.startswith('Alpha'))
print(str.startswith('Bravo'))
print(str.startswith('Bravo', 6))
print(str.endswith('Bravo'))
print(str.endswith('Bravo', 19))
print(str.endswith('Bravo', 19, -1))
```

    True
    False
    True
    True
    True
    False

## 字符串判断系列

```python
# 只包含字母和数字
print('abcd1234'.isalnum())

# 只包含字母
print('abcdABCD'.isalpha())

# 只包含十进制数
print('1234'.isdecimal())

# 只包含数字
print('1234'.isdigit())

# 只包含字母、数字、下划线，且非数字开头
print('_ab01'.isidentifier())

# 只包含小写
print('abcd'.islower())

# 只包含大写
print('ABCD'.isupper())

# 只包含空白字符
print('\n \t \r\n'.isspace())
```

    True
    True
    True
    True
    True
    True
    True
    True

# List 列表

## 读取列表

```python
# 声明一个列表
classmates = ['Alpha','Bravo','Charlie']

# ~len()~ 函数可以获取 list 的元素个数
print(len(classmates))

# 用索引 classmates[n] 来访问特定位置的元素，访问第二个元素 Bravo 为：
print(classmates[1])

# 获取最后一个元素
print(classmates[(len(classmates) - 1)])

# 用复数获取倒数的元素位置
print(classmates[-1])
print(classmates[-2])
```

    3
    Bravo
    Charlie
    Charlie
    Bravo

{% note warning %}  
索引越界会报 IndexError 错，使用 Index 的时候遵循左包右不包的原则。  
{% endnote %}  

## 列表查询

```python
classmates = ['Alpha', 'Bravo', 'Charlie', 'Bravo', 'Alpha']

# 使用 index 查询元素下标，默认从左到右，这里取出左边第一个 Bravo
print(classmates.index('Bravo'))

# 可以定义起始位置，从下标 2 开始往右找
print(classmates.index('Bravo', 2))

# 在 0 到 2 下标里找
print(classmates.index('Bravo', 0, 2))

# 用 count 查询元素个数
print(classmates.count('Bravo'))

# in
if 'Bravo' in classmates:
    print("Bravo is in classmates")
    
if [3, 4] in [1, 2, [3, 4]]:
    print("[3, 4] is in [1, 2, [3, 4]]")
    
for x in classmates:
    print(x, end = " ")
```

    1
    3
    1
    2
    Bravo is in classmates
    [3, 4] is in [1, 2, [3, 4]]
    Alpha Bravo Charlie Bravo Alpha 

{% note info %}  
index() 和 count() 方法的时间复杂度都是 O(n)  
在 list 的的数据结构中直接记录了长度，所以用 len() 取长度时的时间复杂度是 O(1)  
{% endnote %}  

## 修改列表

```python
classmates = ['Alpha','Bravo','Charlie']

# 追加元素到末尾，时间复杂度是 O(1)：
classmates.append('Echo')
print(classmates)

# 插入元素到指定位置：
classmates.insert(3,"Delta")
print(classmates)
# insert 的 index 可以越界，超越上界尾部追加，超越下届头部追加

# 用 pop(index) 删除指定位置的元素：
classmates.pop(3)
print(classmates)

# 当 pop(index) index 为空时删除末尾元素：
classmates.pop()
print(classmates)

# 替换某个位置的元素直接赋值，index 不可以超界
classmates[1] = 'Stone'
print(classmates)

# 删除一个特定的元素
classmates.append('Charlie')
print("Before remove: ", classmates)
classmates.remove('Charlie')
print("After remove: ", classmates)
# 这里只移除了一个 Charlie

# 清空 list
classmates.clear()
print(classmates)
```

    ['Alpha', 'Bravo', 'Charlie', 'Echo']
    ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo']
    ['Alpha', 'Bravo', 'Charlie', 'Echo']
    ['Alpha', 'Bravo', 'Charlie']
    ['Alpha', 'Stone', 'Charlie']
    Before remove:  ['Alpha', 'Stone', 'Charlie', 'Charlie']
    After remove:  ['Alpha', 'Stone', 'Charlie']
    []

{% note info %}  
顺序列表需要主要元素的移动会影响效率和逻辑，类似 insert pop 之类从列表中间添加删除元素的方法应该少用。  
pop 或 clean 大量数据的时候可能会引起垃圾回收，带来效率问题。  
{% endnote %}  

```python
classmates = ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo']
classmates2 = ['Foxtrot', 'Golf', 'Hotel']

# 扩展 list，只接受 iteratable 对象
classmates.extend(classmates2)
print(classmates)

# 反转 list 的所有元素
classmates.reverse()
print(classmates)

# 使用 sort 排序，就地修改
# sort(key=None, reverse=False)
classmates.sort()
print(classmates)

# 降序
classmates.sort(reverse = True)
print(classmates)

# 使用 key 指定排序方法
classmates.extend([1,11,22,2])
classmates.sort(key = str, reverse = True)
print(classmates)
```

    ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo', 'Foxtrot', 'Golf', 'Hotel']
    ['Hotel', 'Golf', 'Foxtrot', 'Echo', 'Delta', 'Charlie', 'Bravo', 'Alpha']
    ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo', 'Foxtrot', 'Golf', 'Hotel']
    ['Hotel', 'Golf', 'Foxtrot', 'Echo', 'Delta', 'Charlie', 'Bravo', 'Alpha']
    ['Hotel', 'Golf', 'Foxtrot', 'Echo', 'Delta', 'Charlie', 'Bravo', 'Alpha', 22, 2, 11, 1]

## 列表的 + \* 运算

```python
lst = []
print(lst * 5)

lst = [1]
print(lst * 5)

lst = [1,2]
print(lst * 5)

lst1 = [1,2,3]
lst2 = [2,3,4]
print("lst1 + lst2 = ", lst1 + lst2)
print("lst2 + lst1 = ", lst2 + lst1)

```

    []
    [1, 1, 1, 1, 1]
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2]
    lst1 + lst2 =  [1, 2, 3, 2, 3, 4]
    lst2 + lst1 =  [2, 3, 4, 1, 2, 3]

{% note info %}  

-   和 \* 都是返回一个新的列表，而 append insert 等都是就地修改。

具体可以在 help() 中看到，返回值为 list 表示为返回新的列表，而返回值为 None 则为就地修改。  
{% endnote %}  

## 列表复制，深浅拷贝

```python
lst0 = list(range(4))
lst1 = list(range(4))

# == 是对值的比较
if lst0 == lst1:
    print("lst0 == lst1")

# is 是对内存地址的比较
if lst0 is lst1:
    print("lst0 is lst2")
else:
    print("lst0 is NOT lst2")
```

    lst0 == lst1
    lst0 is NOT lst2

```python
lst0 = list(range(4))
lst1 = list(range(4))
lst1 = lst0
lst1[2] =10
print(lst0,lst1)

if lst0 is lst1:
    print("lst0 is lst1")
else:
    print("lst0 is NOT lst1")
```

    [0, 1, 10, 3] [0, 1, 10, 3]
    lst0 is lst1

以上代码说明 `lst1 = lst0` 是将 `lst0` 的内存指向赋给了 `lst1`，期间 **并没有复制过程**。复制需要使用 `copy` 方法：  

```python
lst0 = list(range(4))
lst1 = lst0.copy()
print(lst0, lst1)

if lst0 == lst1:
    print("lst0 == lst1")
    
if lst0 is lst1:
    print("lst0 is lst2")
else:
    print("lst0 is NOT lst2")

lst1[2] = 10
print(lst0, lst1)
```

    [0, 1, 2, 3] [0, 1, 2, 3]
    lst0 == lst1
    lst0 is NOT lst2
    [0, 1, 2, 3] [0, 1, 10, 3]

{% note warning %}  
list.copy 有一个坑，在复杂类型拷贝的时候依旧是拷贝的内存地址指向，如下所示  
{% endnote %}  

```python
lst0 = [1, [2, 3, 4], 5]
lst1 = lst0.copy()

if lst0 == lst1:
    print("lst0 和 lst1 的值是相等的")
else:
    print("lst0 != lst1")

# 改变列表中的简单类型
lst1[0] = 5

if lst0 == lst1:
    print("lst0 == lst1")
else:
    print("改变列表中的简单类型，两个列表的值不一样了")

# 再把它改回去
lst1[0] = 1

if lst0 == lst1:
    print("两个列表的值又一样了")
else:
    print("lst0 != lst1")

# 坑在这里：如果改变复杂类型的值
lst1[1][1] = 100
print(lst1)

if lst0 == lst1:
    print("lst0 == lst1 !!!")
else:
    print("lst0 != lst1")
    
# 会发现在仅仅修改 lst1 内的 [2, 3, 4] 之后，lst0 的 [2, 3, 4] 也跟着改变了
print(lst0, lst1, sep="       ")
```

    lst0 和 lst1 的值是相等的
    改变列表中的简单类型，两个列表的值不一样了
    两个列表的值又一样了
    [1, [2, 100, 4], 5]
    lst0 == lst1 !!!
    [1, [2, 100, 4], 5]       [1, [2, 100, 4], 5]

这是因为 list.copy 函数是 `shadow copy` `影子拷贝` 或 `浅拷贝` ，在拷贝列表类的复杂类型时拷贝的依旧是复杂类型内存地址的指向。  

python 也提供了深拷贝：  

```python
# 引入 copy 模块
import copy

lst0 = [1, [2, 3, 4], 5]
lst1 = copy.deepcopy(lst0)

lst1[1][1] = 100

print(lst0, lst1, sep = "        ")
```

    [1, [2, 3, 4], 5]        [1, [2, 100, 4], 5]

通过深拷贝，引用类型的拷贝会复制同样的数据结构到另一个内存地址。  

## 随机数

```python
import random

# 打印十个随机数，取值区间为 0 ~ 1
for i in range(10):
    print(random.randint(0,1), end = " ")
print()

# 随机取出 sequence 里的数据
classmates = ['Alpha', 'Bravo', 'Charlie', 'Delta', 'Echo']
for i in range(5):
    print(random.choice(classmates), end = " ")
print()

# 随机取范围类的随机数，定义方式类似于 range 函数
# 取 100 以内（包含 0 但不包含 100）所有 5 的倍数
for i in range(10):
    print(random.randrange(0,100,5), end = " ")
print()

# 就地打乱列表元素
random.shuffle(classmates)
print(classmates)
```

    1 1 0 1 1 1 1 0 1 1 
    Alpha Alpha Charlie Delta Alpha 
    90 5 95 50 90 5 25 10 45 75 
    ['Bravo', 'Alpha', 'Echo', 'Charlie', 'Delta']

## 列表元素的数据类型

```python
# python 允许列表里有不同的数据类型
me = ['Alpha',1987]
print('My name is {}, born in {}'.format(me[0],me[1]))

# 列表里可以有另一个列表，组成二维、三维、四维数组
s = ['python', 'java', ['asp', 'php'], 'scheme']
print(s[2][1])

# 也可以拆开
p = ['asp', 'php']
s = ['python', 'java', p , 'scheme']
print(p[1], s[2][1])
```

    My name is Alpha, born in 1987
    php
    php php

## tuple 元组

### 定义 tuple

tuple 也是有序列表，称之为 `元组` 。tuple 初始化后就不能修改。声明 tuple 使用圆括号。  
因为不可变，tuple 比 list 更安全，能用 tuple 的时候应当尽量使用 tuple  

```python
# 手动定义 tuple
classmates = ("Alpha", "Bravo", "Charlie")
print(classmates[1], classmates[-1])

# 用 iteratable 可迭代对象建立 tuple
t = tuple(range(1,10,2))
print(t)
```

    Bravo Charlie
    (1, 3, 5, 7, 9)

{% note danger %}  
`t = (1)` 不是一个 tuple，圆括号会被当成运算符处理。  
只有一个元素的 tuple 定义必须加上一个 `~,~` ：  
`t = (1,)`  
{% endnote %}  

{% note info %}  
tuple 的查询方式和 list 基本一致，时间复杂度也一致。由于 tuple 不可变，所以没有增、删、改的操作。  
{% endnote %}  

### “可变的” tuple

```python
t = ("a", "b", ["A", "B"])
t[2][0] = "X"
t[2][1] = "Y"
print(t)
```

    ('a', 'b', ['X', 'Y'])

tuple `t` 只有 `三个` 元素，第三个元素指向了一个 `list` ，而 `list` 是可变的。  
tuple 的不能修改是指其指向不变。要创建一个内容也完全不变的 tuple 应当确保 tuple 的每个元素本身也不能变。  

# if 条件判断

```python
age = 31
if age >= 30:
    print('>30')
elif age >= 20:
    print('<30 & >20')
else:
    print('<20')
```

    >30

{% note default %}  
if 后的判断式只要是非零、非空、非空列表既判断为 `True` ，否则为 `False` 。  
{% endnote %}  

# 循环

## for 循环

```python
# 遍历列表
classmates = ['Alpha', 'Bravo', 'Charlie']
for name in classmates:
    print(name)
```

    Alpha
    Bravo
    Charlie

## range 生成序列

```python
# 使用 range() 生成序列，遵循前包后不包原则
for i in range(3):
    print(i, end=" ")
print()

# range(起点，终点)
for i in range(2, 5):
    print(i, end=" ")
print()

# range(起点，终点，步长)
for i in range(0, 7, 2):
    print(i, end=" ")
print()
```

    0 1 2 
    2 3 4 
    0 2 4 6 

## While 循环

```python
n = 10
sum = 0
while n > 0:
    print(n, end=" ")
    sum += n
    n -= 1
print()
print(sum)
```

    10 9 8 7 6 5 4 3 2 1 
    55

## break 和 continue

```python
# 用 continue 跳过奇数
sum = 0
for i in range(10):
    if i&1:
        continue
    else:
        print(i, end=" ")
        sum += i
print()
print(sum)
```

    0 2 4 6 8 
    20

```python
# 用 break 退出循环
n = 10
while True:
    print(n, end=" ")
    n -= 2
    if n <= 0:
        break
```

    10 8 6 4 2 

# 字典 dict

字典内部的存放顺序和 Key 的放入顺序没有关系，dict 是无序的。  
和 list 相比，dict 快但占用内存较多，是用空间换区时间的一种方法。  

{% note warning %}  
dict 的 key 必须是不可变对象  
{% endnote %}  

```python
# 声明字典
d = {"Alpha":90, "Bravo":80, "Charlie":70}
print(d, d["Alpha"])

# 修改元素
d["Alpha"] = 60
print(d)

# 添加元素
d["Delta"] = 50
print(d)

# 删除元素
d.pop("Delta")
print(d)
```

    {'Alpha': 90, 'Bravo': 80, 'Charlie': 70} 90
    {'Alpha': 60, 'Bravo': 80, 'Charlie': 70}
    {'Delta': 50, 'Alpha': 60, 'Bravo': 80, 'Charlie': 70}
    {'Alpha': 60, 'Bravo': 80, 'Charlie': 70}

```python
d = {"Alpha":90, "Bravo":80, "Charlie":70}

# 判断元素是否存在
print("Alpha" in d, "Echo" in d) # 使用 in 判断
print(d.get("Alpha"), d.get("Echo")) # 使用 dict 对象获取
print(d.get("Echo"), -1) # 不存在时返回自定义 value
```

    True False
    90 None
    None -1

# set

set 和 dict 类似，但只储存 key 不储存 value。  
set 也是无序的，也不可能出现重复的 key。  

```python
# 创建一个 set 需要提供一个 list 作为输入集合
s = set([1,2,3])
print(s)

# 添加元素
s.add(4)
print(s)

# 重复元素会被自动过滤
s = set([1,2,3,1,2,2,3,3])
s.add(3) # 重复添加也会无效
print(s)
```

    {1, 2, 3}
    {1, 2, 3, 4}
    {1, 2, 3}

```python
s1 = set([1,2,3,4,5])
s2 = set([3,4,5,6,7])

# 交集
print(s1 & s2)

# 并集
print(s1 | s2)
```

    {3, 4, 5}
    {1, 2, 3, 4, 5, 6, 7}
