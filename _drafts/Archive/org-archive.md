title: Org-mode Archive 的用法  
date: 2018-10-12  
updated:  
comments: true  
tags:  

-   cheatsheet

layout: post  

---

Archive 功能可以将已完成的任务归档，避免占用屏幕空间，同时还可以配合 org-clock 生成事件记录，是个量化精力的好方法。  

# org-archive 的基本设置

```emacs-lisp
C-h v org-archive-location
```

可以看到预设值为：  

```emacs-lisp
"%s_archive::"
```

在调用 `org-archive-subtree` 时，函数读取这个设置来确定归档位置和方法。这个变量的值分为两个部分，以 `::` 隔开，前半部分表示文件名，后半部分表示标题。  

几个简单的例子：  

```emacs-lisp
;; 如果当前文件名为 projects.org，归档为projects.org_archive（默认值）
"%s_archive::"

;; 归档到当前文件的顶级标题“Archived Tasks”下
"::* Archived Tasks"

;; 归档到 ~/org/archive.org 中
"~/org/archive.org::"

;; 归档到 ~/org/archive.org 中，并以“From 来源文件名”作为标题
"~/org/archive.org:: *From %s"

;; 归档到时间树
"~/org/archive.org::datetree/"
```

`datetree/` 比较特殊，会按日期生成一个时间树，把所有完成的任务按 `完成日期（CLOSED 属性）` 归档。如果任务没有 `完成日期`，则会归档到当前日期。  

最后我自己的设置如下：  

```emacs-lisp
;; Archive to Datetree.org with datetree
(setq org-archive-location "~/OrgCST/Datetree.org::datetree/* %s")
(defun ArchCST/archive-done-tasks ()
  (interactive)
  (org-map-entries 'org-archive-subtree "/DONE"))

(spacemacs/set-leader-keys "oa" 'org-archive-subtree)
(spacemacs/set-leader-keys "oA" 'ArchCST/archive-done-tasks)
```

分别定义了两个 spacemacs 的快捷键，`SPC o a` 用来归档单一的任务，`SPC o A` 用来归档当前文件所有的 `DONE` 项。  

把所有之前完成的任务归档后，我的 `Datetree.org` 文件大概是这样：  

<img src="/uploads/blogpics/org-archive-1.jpg" alt="归档截图" width="" />

# The clock table

我不喜欢直接在 org-agenda-files 中使用 org-clock-report，占用大量的屏幕空间，而别并不方便阅读。  
我以我都是在 archive file 中生成报表，便于观察和检阅。  

在 `Datetree.org` 中创建一个顶级标题，就叫 `REPORTS` 吧，然后在下面直接 `C-c C-x C-r` 调用 `org-clock-report` 即可生成一个基本的报表。  

{% note default %}  
你可能会看到某些日期超出了24小时，导致这个情况出现的原因是可能你连续一周在做一个项目，然后在某一天标记了 `DONE`，然后归档到了 Datetree 的这一天。  

clock table 根据 headings 来计算时间，所以整个项目的时间统计应该会被计算到以完成日期为名称的 heading 下。  
{% endnote %}  

`:maxlevel 3` 可根据需要修改，比如我只想看每个月的报告，不需要精确到天，可以把 `:maxlevel` 修改成 `2`，这样生成的表格中就只精确到二级了。然后按下 `C-c C-c` 重新生成报告，即可看到设置已经生效了。  

<img src="/uploads/blogpics/org-archive-2.jpg" alt="月度报表" width="" />

通过对各种属性的修改，可以实现生成各种各样的报告，这里举几个例子：  

```emacs-lisp
;; Daily report for last 7 days
#+BEGIN: clocktable :scope agenda :maxlevel 0 :tstart
```

对于 Clock table 更详细的设置参数可以在 [这里](https://orgmode.org/manual/The-clock-table.html) 找到。  
