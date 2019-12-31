title: 用好 org-mode 进阶教程（二）：属性  
date: 2018-10-14  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# Properties 的用法

## 什么是 Properties

可以通过设置 `properties` 来表示每一个项的属性。例如 `TODO` 的状态，`SCHEDULED` 的时间，以及写在 `:PROPERTIES:` 抽屉（drawer）中成对的键值（key-value）都属于 `properties`。  

那么 `properties` 就分为了两种：  
`special properties` 是例如 `TODO` `[A]` `SHCEDULED` 这一类的属性，包括 `tags` 也是一种 `special properties`。  
`real properties` 是存放在 `:PROPERTIES:` 中的属性字段，是可以自定义的。  

{% note warning %}  
`special properties` 的值特殊，不要把 `spacial properties` 存放到 `:PROPERTIES:` 中去  
{% endnote %}  

## Real properties 的匹配

例如我有这样一个 Entry：  

```sample
 * This is an entry                :work:
   :PROPERTIES:
   :LOCATION: Office
   :END:
```

在 `C-c a m` 中只需要把 `:LOCATION:`（key）和 `Office`（value）用等号连接起来即可。`value` 一般是字符串，所以需要用**双引号**括起来：  

```sample
LOCATION="Office"
```

如果要同时匹配 location 和 tags，可以这么写：  

```sample
LOCATION="Office"+work
```

## 匹配运算符

如果把 `:LOCATION:` 的键值改为数字 `2`：  

```sample
 * This is an entry                :work:
   :PROPERTIES:
   :LOCATION: 2
   :END:
```

还可以通过这条匹配字符串找到它：  

```sample
LOCATION>1
```

键值并不是静态的，运算符取决于匹配字符串。  

也就是说：  

-   如果匹配字符串中的值是数字，会进行数值匹配，操作符可以为：`<` `=` `>` `<=` `>=` `<>`
-   如果匹配字符串中的值被双引号括起来了，会进行字符串匹配，操作符也可以为上面 6 种，但一般使用 `=` 比较多
-   如果匹配字符串中的值被大括号括起来 `{}`，其中的值被视为正则表达式，只能用 `=` 和 `<>`
-   如果匹配字符串中的值被双引号+尖括号括起来 `~"<>"~` ，其中的值被视为`时间戳`，可以用 6 种操作符

时间戳的用法如下：  

```sample
DEADLINE<="<2018-10-13 11:12>" 表示特定的日期/时间
"<now>" 表示现在，包含的当前的日期和时间
"<today>" 和 "<tomorrow>" 不写明时间就表示今天和明天的 00:00
"<+5d>" 和 "<-2m>" 表示今天后五天内和上两个月到今天，可以用 d(day)、w(week)、m(month)、y(year)
```

[The Org Manual](https://orgmode.org/manual/) 有这么一个比较极端的例子，现在看一看就明白了：  

```
+work-boss+PRIORITY="A"+Coffee="unlimited"+Effort<2+With={Sarah\|Denny}+SCHEDULED>="<2008-10-11>"
```

其中需要注意的一点，`With={Sarah\|Denny}` 正则表达式本来返回的就是字符串，所以并不需要再括起来。其中的 `\` 表示对 `|` 的**再**转义可以证实这一点。  

## Special properties 的匹配

[这里](https://orgmode.org/manual/Special-properties.html#Special-properties) 里有所有的 `special properties`，列出几个常用的：  

| 特殊属性               | 说明                                 |
|---------------------- |------------------------------------ |
| ALLTAGS                | 所有的标签，包括继承的               |
| SCHEDULED              | 计划日期                             |
| DEADLINE               | 截止日期                             |
| CLOSED                 | 完成时间                             |
| TIMESTAMP              | 第一个没有 key 的时间戳              |
| TIMESTAMP<sub>IA</sub> | 第一个未激活的时间戳`[]`，通常用来标记录入时间 |
| TODO                   | TODO 的状态，如果设置了多个 TODO 和 DONE 的状态需要使用 |
| FILE                   | 所属文件名                           |
| ITEM                   | 匹配 Entry 标题文本                  |

还有一个最特殊的 key，`LEVEL`。 这个 key 表示 Entry 的等级。例如：  

```sample
;; 筛选所有三级 Entry
LEVEL=3

;; 筛选所有三级标题中含有 boss 标签且未完成的 Entry
LEVEL=3+boss-TODO="DONE"
```

我们把上面那个 Entry 拿来再改一下：  

```sample
 * WIP [#B] This is an entry                       :work:
   DEADLINE: <2018-10-20 Sat> SCHEDULED: <2018-10-13 Sat>
   :PROPERTIES:
   :LOCATION: 2
   :END:
   [2018-10-13 Sat 14:20]
```

这就是一个比较完整的 Entry 了。它具有 `TODO` `PRIORITY` `ALLTAGS` `DEADLINE` `SCHEDULED` `TIMESTAMP_IA` 这几个 `special properties`，和 `real properties` 相比，检索上有什么区别呢？  

没有区别。除了 `ALLTAGS` 是预设的不需要输入 key。  

例如：  

```sample
;; 检索创建这周创建的 Entry
TIMESTAMP_IA>"<-1w>"

;; 检索五天内要到期的 Entry
DEADLINE<"<+5d>"

;; 检索优先级为B的 Entry
PRIORITY="B" ;; 这里似乎出问题了？
```

在检索 PRIORITY 的时候发现 B 优先级似乎包含了所有没有标记优先级的任务，使用 `C-h v` 查询变量 `org-default-priority` 发现值为 `66`，而：  

```python
# python code
print("What is the ASCII code for 'B'? It is {}!".format(ord("B")))
```

    What is the ASCII code for 'B'? It is 66!

所以 org-mode 默认的优先级就是 B 级，所以在检索中出现优先级为 B 级时一定会包含所有没有设置优先级的 Entry。  
要避免这种情况出现，可以将 `org-default-priority` 设为 `90` ，也就是 `Z` 级。  

## Properties 的继承

如果父级 Entry 设定了 property，子 Entry 确实是可以继承的，只是默认值没有开启这个功能。官方明确声明了 properties 的继承会 `显著` 提高 properties 的搜索时间。  

如果需要打开 properties 的继承功能，可以这样设置：  

```emacs-lisp
(setq org-use-property-inheritance t)
```

{% note danger %}  
虽然可以这么设置，但多余的搜索耗时没有必要，应当遵循官方的意见。  
{% endnote %}  

## 新建标题自动插入属性

```emacs-lisp
(add-hook 'org-insert-heading-hook
          (lambda () (org-set-property "CREATED" (format-time-string "%Y/%m/%d %H:%M"))))
```
