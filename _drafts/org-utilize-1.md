title: 用好 org-mode 进阶教程（一）：标签  
date: 2018-10-12  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

关于 org-mode 的使用网络上有大量的文章，讲述了各种功能的用快捷键等等，但没有一篇是从实际应用出发。  
本系列属于 org-mode 的进阶教程，方方面面讲解了 org-mode 的各种强大的功能应该如何实际的使用，从 Tags 和 Properties 应该如何区分，到 org-mode 三大维度的结构设计等等。  
本系列例子具有强烈的个人色彩，实际应用上读者的配置不应该和博主的配置完全一样，每个人应当都有自己的理解，博主希望能授之以渔。  
博主从 13 年 Ominifocus 2 时代开始接触 GTD，虽然无法和其他大神相比，但几乎尝试过市面上所有 TODO 类软件。通过不断的修改结构设计，有所心得。  
阅读本系列希望读者应该熟悉 org-mode，至少已经有过一个月以上的使用经验。  

# 标签 Tags

## Tags 基本使用

### Tags 的基本用法

org-mode 使用每个 Headline 尾部以 `::` 括起来的内容作为标签，可以直接在 Heading 尾部输入 `:work:` 来设置标签，也可以使用 `C-c C-c` 使用 `org-set-tags-command` 快速生成标签。  
默认情况下 `C-c C-c` 包含了当前 buffer 中所有的标签。也可以使用自己预设的标签。  

### TODO 预设 Tags

有两种方式预设 Tags。可以在每个文件的头部信息添加预设 Tags，也可以用 `init.el` 配置文件设置。这两种方式有个区别，文件头部预设的 Tags 只在当前文件可以通过 `C-c C-c` 快速添加，而配置文件设置的 Tags 可以全局使用。  
因为可以单独给每个文件配置它们特有的 Tags，所以全局配置最好是设置一些比较通用的 Tags，这样在管理上比较方便，同时对以后生成 Agenda 和 Clock table 有很大的帮助。  

1.  TODO 文件头部添加预设 Tags

    只需要在 org-file 的文件头部写入 `#+TAGS:` 加上需要预设的 Tags 就行了。例如：  
    
    ```sample
    #+TAGS: { home(h) work(w) tennisclub(t) }
    #+TAGS: laptop(l) car(c) pc(p) sailboat(s)
    ```
    
    `{}` 括起来表示单选，这三个里面只能选择一个。圆括号 `()` 里表示的是快捷键。  
    如果没有设置快捷键，org-mode 会自动分配未使用的快捷键给它们。  
    
    在这两行上使用 `C-c` 可以立即刷新配置。  

2.  TODO 配置文件预设 Tags

    添加 `org-tag-persistent-alist` 到配置文件中，我的配置如下：  
    
    ```emacs-lisp
    (setq org-tag-persistent-alist '(("FLAGGED" . ?1) ("RAW" . ?0) ("ROUTINE" . ?8) ("REVIEW" . ?9)
                                     (:newline)
                                     ("FULFILMENT" . ?f) ("WORK" .?j) ("EARNED" . ?e) ("WASTED" . ?w)
                                     (:newline)
                                     ("IMPORTANT" . ?i) ("URGENT" . ?u)
                                     (:newline)))
    ```
    
    稍微解释一下我的配置，  

### TODO Tags 的分组

分组的原则是把 `（grouptag）组名标签` 看做是所有子标签和子组的分类。  
搜索 `组名标签` 的时候会返回所有子标签。反过来则不行。  
例如我设计了这么一个分组：  

```sample
#+TAGS: [ emacs : spacemacs elisp ]
```

期望表现为当一个任务为配置 spacemacs 的时候我打 spacemacs 标签，而写自己的 .el 包的时候打 elisp 标签，原则上不使用 emacs 这个组名标签，否者逻辑非常容易搞混。  
这样当我不想写代码的时候可以搜索 spacemacs 标签，进行一些简单的配置。而有精力有时间的时候搜索 elisp 标签改进一下代码。想找到所有和 emacs 相关的任务时搜索 emacs 标签。  

一个错误的示例：  

```sample
#+TAGS: [ allplaces : office home ]
```

office 和 home 属于 allplaces 逻辑上似乎没有问题。  
那么有一个任务设置为 `查看邮件 :allplaces:` 表示在哪都能查看邮件，不限定是在 home 还是在 office，甚至地铁上也可以用手机查看邮件，逻辑上似乎还是没有问题。  
当你在地铁上，想看一下 allplaces 能做啥，结果出来了，含有 `查看邮件 :allplaces:` 没毛病。  
但当你在 office ，想要搜索我在 office 能干啥，这时候搜索结果里**并不会**包含 `查看邮件 :allplaces:` 这个任务！  

所以一般不要轻易使用组名标签，因为它只是一个分类。理解为家里有爸妈，搜妈不出家。  

### TODO Tags 继承的匹配方式

但是组名标签就完全不用吗，也不是。  
Tags 分组是具有继承特征的。根据这个特性，组名标签使用在 `文件结构` 中，可以达到分类和量化的目的。  
例如我去年做个一个 Global 的项目 `Migration to Windows 10` 打了 `:TEAM_IT:` 的标签，此标签的设计如下：  

```sample
#TAGS: { TEAM_IT : Japan Australia Mainland India }
#TAGS: [ Japan : Shibue ]
#TAGS: [ Australia : Mary Martin ]
#TAGS: [ Mainland : Bruce ]
#TAGS: [ India : Ashutosh Sajid Devandra]
```

我的文件结构为  

```sample
 * PROJECTS
 ** Migration to Windows 10            :TEAM_IT:
 *** China                            :Mainland:
 **** Chengdu                            :Bruce:
 **** Beijing                            :Bruce:
 **** Shanghai                           :Bruce:
 *** Australia                       :Australia:
 **** Sydney                              :Mary:
 **** Newcastle                         :Martin:
```

这样在 Clock table 和 agenda 从就能通过严密的逻辑去查找个量化了。  

## Tags 的检索

使用 `SPC a o m` （emacs `C-c a m`）调用 `org-tags-view` 后可以直接输入标签进行检索，可显示所有含有此 标签的项（Entry）。  

使用 `SPC u SPC a o m`（emacs `C-c a M`）调用 `org-tags-view` 后输入标签显示的是所有含此标签的 `TODO` 项。如果你设置了多个 `TODO` 的状态，那么都会显示出来。  

输入的内容叫 `匹配字符串`，`org-tags-view` 将拿这个匹配字符串到 `agenda-files` 中去匹配满足 `匹配字符串` 条件的 Entry 并显示出来。  

## Tags 的匹配语法

布尔操作符 `&` 和 `|` 可以使用，分别表示的是 `AND` 和 `OR`。  

`&` 的优先级高于 `|`  

几个例子：  

```sample
;; 搜索所有包含 :work: 的项
work

;; 搜索同时具有 :work: 和 :boss: 的项
work&boss

;; 搜索只有 :work: 没有 :boss: 的项
work-boss  ;; 在 +- 号出现时可以省略 &
+work-boss

;; 搜索有 :work: 或 :boss: 的项
work|boss

;; 搜索有 :work: 或 :laptop+night: 的项：
work|laptop+night
```

最后一行注意，因为有 `+` 的存在省略了 `&`，又因为 `&` 优先级较 `|` 高，所以是匹配 `work` 或者 `laptop+night` 而不是匹配 `work 或 laptop` 再加 `night`。  

{% note info %}  
`org-tags-view` 不支持圆括号 `()` 的使用。  
{% endnote %}  

## Tags 的匹配支持正则表达式

Tags 的匹配支持用大括号 `{}` 括起来的正则表达式，例如标签中有 `:bosszhang:` `:bossli:`，可以用如下表达式一起筛选出来：  

```sample
work+{^boss.*}
```
