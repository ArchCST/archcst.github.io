title: 用好 org-mode 进阶教程（三）：结构设计  
date: 2018-10-14  
updated:  
comments: true  
tags:  

-   learn
-   zen

layout: post  

---

# 三大维度的特性

## 第三大维度，文件

## 总结三大维度的特性

文件的特点是结构清晰，用来整理项目。  
Tags 方便检索，特别是重复的地点人物。  
Properties 则是对两者的补充。  

# 根据特性设计结构

必须要注意的一点是**节制**。  

## 文件

按角色分。  
比如我的角色有：公司员工，家庭成员，自我。  

自我分两类，一类包含所有例行任务（纪念日，充值话费等）和杂务（洗衣，取快递等）。  
另一类则是 Fulfilment 自我价值实现。  

所以我的整个 org-mode 文件结构大概是这样的：  

```sample
Work.org
|--MISCELLANEOUS
|--PROJECTS
|--JOB TASKS


Personal.org
|--MISCELLANEOUS
|--FAMILY
|--HUSBAND & FATHER
|--SOCIAL RELATIONS
|--ENTERTAINMENT
|--SHOPPING

Fulfilement.org
|--LEARN
|--CREATION
|--TRAINING
|--HEALTH
|--FINANCIAL
|--IDEA
```

在设计文件结构的时候一定要注意节制，能合并的就不要分开。  
虽然 org-mode 允许无限层级，但最好只允许两级标题大写，表示为分类。第三级往下是具体的项目和任务，这样在筛选和量化的时候会方便很多。  

## 标签

Agenda 对标签的支持是最好的，也是检索最快的，继承性用起来也非常方便。所以标签用来做为特别重要的分类最为合适。  

### Global 标签

首先是 Global 的配置，这里面我会放上基本上必须得选的配置，放到 `org-tag-persistent-alist` 中  
FLAGGED ROUTINE  
VALUABLE WORK WASTED  

本着节制的原则，没有设计 Arrival、Leaving 而只是用 ONTHEGO 来标记所有的地点切换，包括路上需要做的事，全部归于 ONTHEGO。  

### 文件头的标签

文件头的标签则是用来设计当前文件特别含有的标签。  
比如 work.org，可能需要设计组员的名称，方便在 WAITING 的情况下定位。我的三个文件设计如下：  

```sample
;; Work.org
#+TAGS: [ LEADERS : Ben Ashutosh ]
#+TAGS: [ COLLEAGUES : Dennies Norman Carter ]
#+TAGS: [ VENDERS : Cisco IBM Dell Apple ]
#+TAGS: [ TEAMS : Network HelpDesk ]

;; Personal.org
#+TAGS: 

```

## 属性

### Special properties

TODO 的状态设置：TODO NEXT PENDING WAITING SOMEDAY  
DONE 的状态设置：DONE ABORT  

可以自定义 TODO 的颜色，以便于区分：  

```emacs-lisp
(setf org-todo-keyword-faces '(("TODO" . (:foreground "white" :weight bold))
                               ("NEXT" . (:foreground "white" :weight bold))
                               ("PEDING" . (:foreground "#CCCCCC" :weight bold))

))
```

### Real properties

VALUABLE：-10 -5 0 5 10  
REFERENCE：  
LOCATION  
DEVICE  

# 清空历史记录

进行了大量的测试性质的检索后，正常使用的时候会造成干扰。  
历史记录存放在 `org-tags-history` 里，可以自行定义一个函数清空它。  

```emacs-lisp
;; Cleanup search temp
(defun ArchCST/cleanup-org-tags-history()
  "This function will reset `org-tags-history' value to nil."
  (interactive)
  (setq org-tags-history nil))
```
