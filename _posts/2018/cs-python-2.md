title: Cheatsheet Python3 - 函数  
date: 2018-10-06  
updated:  
comments: true  
tags:  

-   cheatsheet

layout: post  

---

Cheatsheet 类博文详情请见 [关于页面](https://archcst.me/about/)。  
学习资料：[Python教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)，[Python编程-函数基础](https://ke.qq.com/webcourse/index.html#cid=134017&term_id=100150034&taid=1982410875079553&vid=s1424vgil4m)  

# 函数查询

在这里可以查到 Python 3 的内建函数文档  
[Built-in Functions — Python 3.7.1rc1 documentation](https://docs.python.org/3/library/functions.html)  

可以使用 `help(function name)` 查看帮助信息  

<!--more-->

```python
print(abs(-2.5)+abs(1))
help(abs)
```

    3.5
    Help on built-in function abs in module builtins:
    
    abs(x, /)
        Return the absolute value of the argument.

<!-- more -->

# 内置函数

## 数据类型转换

```python
print(int('123'))
print(int(12.3))
print(float('12.3'))
print(str(12.3))
print(bool(1), bool(0), bool(-1), bool(12.5), bool(-12.5),bool(""), bool(" "))
```

    123
    12
    12.3
    12.3
    True False True True True False True

## 进制转换

```python
print(hex(128))
print(bin(128))
print(oct(128))
```

    0x80
    0b10000000
    0o200

```python
print(0x80)
print(0b10000000)
print(0o200)
```

    128
    128
    128

# 函数定义

函数的定义方式：  

```sample
def 函数名(参数列表):
    函数体
    [return 返回值]
```

1.  函数名就是标识符，命名要求一样
2.  语句块必须缩进，约定为四个空格
3.  如果函数没有 return 语句，则隐式返回一个 None 值，所以其实 Python 中所有函数都有返回值。
4.  定义中的参数列表成为形式参数，只是一种符号表达，简称**形参**
5.  相对于形参，调用函数过程中传入的参数是实实在在传入的值，简称**实参**

```python
# 使用 def 定义函数，依次写出函数名、括号和参数、冒号
def my_abs(x):
    # 在缩进块中写函数体
    if x >= 0:
        # 可以使用 pass 来占位，稍后填写
        # 如果不写 pass 函数调用会出错
        pass
    else:
        # 用 return 返回返回值
        return -x
    
# 调用函数
print(my_abs(12.3)) # 无返回值，默认隐式返回 None
print(my_abs(-12.3))
```

    None
    12.3

如果将函数保存在 `myabs.py` 文件中，使用 `import` 来导入函数  

```python
# 注意不写 myabs.py 的后缀名 .py
from myabs import my_abs
```

# 返回多个值

```python
import math
# 计算坐标，参数为x坐标、y坐标、步长、角度
def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    # 返回新的x坐标、y坐标
    # 实际上返回的是一个 tuple，但语法上可以省略括号
    return nx, ny

# 返回值是一个 tuple
r = move(100, 100, 60, math.pi / 6)
print("returned tuple =", r)

# 也可以直接把返回的 tuple 按位置赋值给变量
x, y = move(100, 100, 60, math.pi / 6)
print("x = {}, y = {}".format(x, y))
```

    returned tuple = (151.96152422706632, 70.0)
    x = 151.96152422706632, y = 70.0

# 函数的参数

## 位置参数：按顺序赋值

位置参数调用时必须匹配个数，同时按照参数定义的顺序传入实参  

```python
# 计算 x 的 n 次方 
def power(x, n):
    ans = 1
    for i in range(n):
        ans *= x
    return ans

print(power(5,3))
```

    125

## 默认参数：设定参数的默认值

直接在形参上写入默认值（缺省值）即为默认参数：  

```python
# 上一个 power 如果不写 n 的值，默认设定为 2
def power(x, n = 2):
    ans = 1
    while n > 0:
        n -= 1
        ans *=x
    return ans

print(power(5))
print(power(5, 5))
```

    25
    3125

{% note info %}  
根据逻辑，必然需要设置 **必选参数在前** ，**可选（默认）参数在后**。  
{% endnote %}  

不按顺序设置参数，跳过默认参数：  

```python
def classmates(name, age = 31, city = "Chengdu"):
    print("Name:{: <8}".format(name), end=" ")
    print("Age:{: <3}".format(age), end=" ")
    print("City:{: <15}".format(city), end=" ")
    print()

# 使用默认年龄和地址
classmates('Alpha')
# 自定义年龄和地址
classmates('Bravo', 28, "Chongqing")
# 跳过设置年龄
classmates('Charlie', city="Beijing")
# 先设置地址再设置年龄
classmates('Delta', city="Shenzhen", age=16)

```

    Name:Alpha    Age:31  City:Chengdu         
    Name:Bravo    Age:28  City:Chongqing       
    Name:Charlie  Age:31  City:Beijing         
    Name:Delta    Age:16  City:Shenzhen        

{% note default %}  
定义的默认参数应该指向不可变对象，Unless you know what you're doing.  
{% endnote %}  

## 可变参数

在位置参数中，可以把参数作为一个 tuple 或 list 传进来，但调用的使用必须先组装出一个 tuple 或 list：  

```python
# 计算 a^2 + b^2 + c^2 + ……
def calc(numbers):
    sum = 0
    for i in numbers:
        sum += i * i
    return sum

# 用 list 带入
n1 = calc([2, 3, 4])
# 用 tuple 带入
n2 = calc((2, 3, 4))
print(n1, n2)
```

    29 29

在形参前使用 `*` 表示该形参是可变参数，可以接收多个实参。 Python 收集多个实参为一个 tuple 。  

```python
# 添加 * 表示该形参是可变参数
def calc(*numbers):
    sum = 0
    for i in numbers: # 在函数体内不要再加 *
        sum += i * i
    return sum

# 即可实现调用时无需组装list 或 tuple
n1 = calc(2, 3, 4)
# 也可以调用 0 个参数
n2 = calc()
print(n1, n2)

# 如果已经有一个 tuple 或 list
nums = [1, 2, 3]
# 可以使用元素位置
print(calc(nums[0], nums[1], nums[2]))
# 也可以在 tuple 或 list 前面加上 *，把 list 或 tuple 的元素变成可变参数传进去
print(calc(*nums))
```

    29 0
    14
    14

{% note info %}  
在 `numbers` 前面加上 `*` 后，在函数内部，参数 `numbers` 接收到的是一个 tuple。  
{% endnote %}  

## 关键字参数

关键字参数允许传入任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个 dict  
形参前使用 `**` ，表示可以接受多个关键字参数（可变的关键字参数）  

```python
def person(name, age, **kw):
    print('Name: {: <8} Age: {: <3} Other:{}'.format(name, age, kw))

# 只传入必选参数
person('Alpha', 31)

# 传入一个关键字参数“city”
person('Bravo', 30, city='Chengdu')

# 传入多个关键字参数“gender”和“job”
person('Charlie', 29, gender='M', job='Engineer')

# 关键字参数也可以先组装出一个 dict 再传入
bjengineer = {'city': 'Beijing', 'job': 'Engineer'}
person('Delta', 28, city=bjengineer['city'], job=bjengineer['job'])
# 可以简化为
person('Echo', 27, **bjengineer)
```

    Name: Alpha    Age: 31  Other:{}
    Name: Bravo    Age: 30  Other:{'city': 'Chengdu'}
    Name: Charlie  Age: 29  Other:{'job': 'Engineer', 'gender': 'M'}
    Name: Delta    Age: 28  Other:{'city': 'Beijing', 'job': 'Engineer'}
    Name: Echo     Age: 27  Other:{'city': 'Beijing', 'job': 'Engineer'}

{% note info %}  
`**bjengineer` 表示把这个字典的所有 key-value 用关键字传入到函数的 `**kw` 参数，`kw` 将获得一个 `bjengineer` 字典的拷贝，对 `kw` 的改动不会影响到函数外的 `bjengineer`  
{% endnote %}  

关键字参数可以传入 `不受限` 的关键字，需要检查可以在函数内部通过 `kw` 检查  

```python
# 检查传入的关键字参数
def person(name, age, **kw):
    if 'city' not in kw:
        print('{} may need a place to live!'.format(name))
    if 'job' in kw:
        print('{} has a Job! Work as {}'.format(name, kw['job']))

person('Alpha',31,job='Engineer')
```

    Alpha may need a place to live!
    Alpha has a Job! Work as Engineer

## keyword-only 参数（命名关键字参数）

虽然关键字参数可以对传入的关键字进行检查，但仍旧可以传入不受限的关键字。  
除了对关键字进行检查，可以使用 `命名关键字参数` 限制传入的关键字名称。  

```python
# “*” 为 keyword-only 参数的特殊分隔符，“*” 后面的参数被视为 keyword-only
def kwonly(*, x, y):
    print(x, y)

kwonly(x = 5, y = 10)
# kwonly(5, 10) 是错误的调用方式，必须使用 key-value 键值对

# 只接受“city”和“job”的命名关键字参数
def person(name, age, *, city, job):
    print('Name: {: <8} Age: {: <3} City: {: <10} Job: {: <10}'.format(name, age, city, job))

person('Alpah', 29, city='Beijing', job='Engineer')
person('Bravo', 27, job='Engineer', city='Beijing')

```

    5 10
    Name: Alpah    Age: 29  City: Beijing    Job: Engineer  
    Name: Bravo    Age: 27  City: Beijing    Job: Engineer  

{% note danger %}  
命名关键字参数必须传入形参名，否则报错，例如  
`person('Charlie', 30, 'Beijing', 'Engineer')`  
是错误的用法  
{% endnote %}  

```python
# 如果函数已经有一个“可变参数”，后续的命名关键字参数不再需要特殊分隔符“*”，自动视为 keyword-only 参数
# worktime 传入工作的年月日三个参数，计算工作了多少天（假设每个月30天，无闰年）
def person(name, age, *worktime, city, job):
    print('Name: {: <8} Age: {: <3} City: {: <10} Job: {: <10}'.format(name, age, city, job), end=" ")
    sum = worktime[0] * 356 + worktime[1] * 30 + worktime[2]
    print('Worktime:', sum)

person('Alpha', 31, 1, 2, 3 ,city='Beijing', job='Engineer')
```

    Name: Alpha    Age: 31  City: Beijing    Job: Engineer   Worktime: 419

```python
# 可以给命名关键字参数设置缺省值来简化调用
def person(name, age, *, city='Beijing', job):
    print('Name: {: <8} Age: {: <3} City: {: <10} Job: {: <10}'.format(name, age, city, job))

person('Alpha', 31, job='Engineer')
```

    Name: Alpha    Age: 31  City: Beijing    Job: Engineer  

## 参数组合

{% note info %}  
组合使用参数时的一般顺序是：  

1.  普通参数
2.  缺省参数
3.  可变位置参数
4.  keyword-only 参数（可带缺省值）
5.  可变关键字参数

虽然可以组合多种参数，但为函数接口的可理解性，尽量不要同时使用过多的组合。  
{% endnote %}  

```python
# 包含四种参数的函数（没有可变参数，需要使用特殊分隔符）
def test4types(a, b=0, *, op, **kw):
    print('a={}   b={}   Operator: {}   Other:{}'.format(a, b, op, kw))

test4types(1, op='Alpha', location='Chengdu')

# 包含 5 种参数的函数（因为有可变参数所以参数列表中没有特殊分隔符）
def test5types(a, b=0, *calcsum, op, **kw):
    sum = 0
    for i in calcsum:
        sum += i
    print('a={}   b={}   Calcsum={}   Operator: {}   Other:{}'.format(a, b, sum, op, kw))

test5types(1, 2, 10, 10, 10, op='Bravo', time='today')
```

    a=1   b=0   Operator: Alpha   Other:{'location': 'Chengdu'}
    a=1   b=2   Calcsum=30   Operator: Bravo   Other:{'time': 'today'}

{% note info %}  
通过 Tuple 和 dict 也可以调用上述函数  
{% endnote %}  

```python
def test4types(a, b=0, *, op, **kw):
    print('a={}   b={}   Operator: {}   Other:{}'.format(a, b, op, kw))

def test5types(a, b=0, *calcsum, op, **kw):
    sum = 0
    for i in calcsum:
        sum += i
    print('a={}   b={}   Calcsum={}   Operator: {}   Other:{}'.format(a, b, sum, op, kw))

# 通过 tuple 和 dict 调用 test4types
t1 = (1, ) # 这里注意一个元素的 tuple 也必须有“,”，否则会按照计算处理
d1 = {'op': 'Alpha', 'location': 'Bejing', 'time': 'Today'}
test4types(*t1, **d1)

# 通过 tuple 和 dict 调用 test5types
t2 = (1, 2, 10, 10, 10)
d2 = {'op': 'Bravo', 'location': 'Chegndu', 'time': 'Today'}
test5types(*t2, **d2)

```

    a=1   b=0   Operator: Alpha   Other:{'location': 'Bejing', 'time': 'Today'}
    a=1   b=2   Calcsum=30   Operator: Bravo   Other:{'location': 'Chegndu', 'time': 'Today'}

## 总结

### 可变参数

可变参数分为 `位置可变参数` 和 `关键字可变参数`  
`位置可变参数` 在形参前使用 `*` ，收集成 `tuple`  
`关键字可变参数` 在形参前使用 `**` ，收集成 `dict`  
`位置可变参数` 和 `关键字可变参数` 都是用用于收集可变数量的实参。  

# 参数解构

概念：给函数提供**实参**的时候，可以在集合类型前使用 `*` 或 `**`，把集合类型的结构解开，提取出所有元素作为函数的实参。  
非字典类型使用 `*` 解构成 `位置参数`  
字典类型使用 `**` 解构成 `关键字参数`  

{% note info %}  
提取出来的元素数目要和参数的要求匹配，也要和参数的类型匹配。  
{% endnote %}  

```python
def add(x,y):
    result = x + y
    print(result)

t = (10, 20)

# 直接使用 add(t) 等于使用 add((1,2))，只传入了一个参数，所以会报错。
# 可以如下使用下标分别传入
add(t[0], t[1])

# 也可以对 tuple 进行“参数解构”
add(*t)

# 对可迭代对象进行参数解构
add(*range(1,3))

# 对 list 进行参数解构
add(*[2,5])
```

    30
    30
    3
    7

对字典的解构：  

```python
def add(x,y):
    result = x + y
    print(result)

# 解构字典，因为形参是 x 和 y，字典的内容必须是 x 和 y，相当于关键字传参
d = {'x' : 5, 'y' : 6}
add(**d) # 相当于 add(x=5, y=6)

# 也可以把字典解构成 keys 或 values 组成的可迭代对象，再传入
d1 = {'x1' : 7, 'y1' : 8}
add(*d1.keys())   # 相当于 add('x1', 'y1')
add(*d1.values()) # 相当于 add(7, 8)

```

    11
    x1y1
    15

# 递归函数

在一个函数内部调用自身本身，这个函数就是递归函数  

```python
# 计算阶乘
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)

print(fact(5))
```

    120

递归调用的次数过多会导致栈溢出，报错如下：  

```sample
RuntimeError: maximum recursion depth exceeded in comparison
```
