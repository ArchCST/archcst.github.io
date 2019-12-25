title: Spacemacs Org-mode 快捷键中文说明书  
date: 2018-09-01  
updated: 2018-09-16  
comments: true  
tags:  

-   learn
-   emacs

layout: post  

---

复制至 Spacemacs org-layer 的 README，翻译了快捷键部分  

从 emacs 迁移到 spacemacs，用 evil 模式也用了一段时间了，对于在 evil 模式下 org-mode 快捷键一直一知半解，所以把 README 拿出来全方位的过一遍，顺便看看还有哪些好用的功能没用上。  
一边试每一个快捷键一边翻译， 时间关系，不太常用的或者只靠试键搞不懂用处的暂时先不翻了，以后有空再继续，希望不要烂尾。  
spacemacs 里就有原文，在这里能找到（需要先安装 spacemacs）：  

<!--more-->

```shell
~/.emacs.d/layers/+emacs/org/README.ORG
```

<!-- more -->

# 按键绑定 Key bindings

## 开始使用 org-mode

| 按键绑定      | 说明                                 |
|------------- |------------------------------------ |
| `SPC a o #`   | org agenda list stuck projects       |
| `SPC a o /`   | org occur in agenda files            |
| `SPC a o a`   | 日程列表 org agenda list             |
| `SPC a o c`   | 快速创建项 org capture               |
| `SPC a o e`   | org store agenda views               |
| `SPC a o f i` | org feed goto inbox                  |
| `SPC a o f u` | org feed update all                  |
| `SPC a o k g` | 进入最后一次始计时的项               |
| `SPC a o k i` | 继续计时之前最后一次停止的项 org clock in last |
| `SPC a o k j` | 进入当前开始计时的项 org jump to current clock |
| `SPC a o k o` | 停止当前计时 org clock out           |
| `SPC a o k r` | org resolve clocks                   |
| `SPC a o l`   | org store link                       |
| `SPC a o m`   | 以标签进行搜索 org tags view         |
| `SPC a o o`   | 日程列表选单 org agenda              |
| `SPC a o s`   | 正则搜索 org search view             |
| `SPC a o t`   | 显示所有剩余的 TODO 项 org todo list |
| `SPC C c`     | 快速创建项 org-capture               |

## 开关 Toggles

| 按键绑定    | 说明                                                         |
|----------- |------------------------------------------------------------ |
| `SPC m T c` | 改变当前项 checkbox 的勾选状态 org-toggle-checkbox           |
| `SPC m T e` | org-toggle-pretty-entities                                   |
| `SPC m T i` | org-toggle-inline-images                                     |
| `SPC m T l` | 显示当前文件所有超链接的地址 org-toggle-link-display         |
| `SPC m T t` | 显示当前文件所有 TOTO 项 org-show-todo-tree                  |
| `SPC m T T` | 切换 TODO 的状态 org-todo                                    |
| `SPC m T V` | 切换当前文件到只读的阅读模式 toggle `space-doc-mode` a read-only view mode |
| `SPC m T x` | 预览 latex org-preview-latex-fragment                        |

## Org-mode

| 按键绑定                                     | 说明                                                                  |
|-------------------------------------------- |--------------------------------------------------------------------- |
| `SPC m <dotspacemacs-major-mode-leader-key>` | org-ctrl-c-ctrl-c                                                     |
| `SPC m *`                                    | 正文转标题，或标题转正文 org-ctrl-c-star                              |
| `SPC m RET`                                  | org-ctrl-c-ret                                                        |
| `SPC m -`                                    | 修改列表项符号 org-ctrl-c-minus                                       |
| `SPC m '​`                                    | org-edit-special                                                      |
| `SPC m a`                                    | 日程列表选单 org-agenda                                               |
| `SPC m A`                                    | 插入附件 org-attach                                                   |
| `SPC m c`                                    | 快速录入 org-capture                                                  |
| `SPC m C c`                                  | 取消当前计时 org-clock-cancel                                         |
| `SPC m C g`                                  | 重新计算计时 evil-org-recompute-clocks                                |
| `SPC m C i`                                  | 在当前项开始计时 org-clock-in                                         |
| `SPC m C o`                                  | 结束当前计时 org-clock-out                                            |
| `SPC m C r`                                  | org-resolve-clocks                                                    |
| `SPC m d d`                                  | 为当前项添加截止时间 org-deadline                                     |
| `SPC m d s`                                  | 为当前项添加排程时间 org-schedule                                     |
| `SPC m d t`                                  | 为当前项添加时间戳 org-time-stamp                                     |
| `SPC m d T`                                  | 为当前项添加未激活的时间戳 org-time-stamp-inactive                    |
| `SPC m e e`                                  | 导出当前文件 org-export-dispatch                                      |
| `SPC m e m`                                  | 导出当前文件为一个 html 格式的 email 消息 send current buffer as HTML email message |
| `SPC m f i`                                  | org-feed-goto-inbox                                                   |
| `SPC m f u`                                  | org-feed-update-all                                                   |
| `SPC m l`                                    | org-open-at-point                                                     |
| `SPC m L`                                    | org-shiftright                                                        |
| `SPC m H`                                    | org-shiftleft                                                         |
| `SPC m K`                                    | org-shiftup                                                           |
| `SPC m J`                                    | org-shiftdown                                                         |
| `SPC m C-S-l`                                | org-shiftcontrolright                                                 |
| `SPC m C-S-h`                                | org-shiftcontrolleft                                                  |
| `SPC m C-S-j`                                | org-shiftcontroldown                                                  |
| `SPC m C-S-k`                                | org-shiftcontrolup                                                    |
| `SPC s j`                                    | 跳到一个标题 spacemacs/jump-in-buffer (jump to a heading)             |

## Org with evil-org-mode

Please see the [evil-org documentation](https://github.com/Somelauw/evil-org-mode/blob/master/doc/keythemes.org) for additional instructions on customizing  
`evil-org-mode`.  

| 按键绑定      | 说明                                      |
|------------- |----------------------------------------- |
| `gj` / `gk`   | 上一个/下一个标题 Next/previous element (heading) |
| `gh` / `gl`   | 上一级/下一级标题 Parent/child element (heading) |
| `gH`          | 当前项的顶级标题 Root heading             |
| `ae`          | Element text object                       |
| `ar`          | Subtree text object                       |
| `M-j` / `M-k` | 上下移动当前树，联动子树 Move heading     |
| `M-h` / `M-l` | 升降级当前树，不联动子树 Promote or demote heading |
| `M-J` / `M-K` | 上下移动当前树，不联动子树 Move subtree   |
| `M-H` / `M-L` | 升降级当前树，联动子树 Promote or demote subtree |
| `>>` / `<<`   | 升降级当前树，不联动子树 Promote or demote heading |
|               |                                           |

如果启用了 `org-want-todo-bindings` ，以下快捷键可用  

| 按键绑定s | 说明                                |
|----- |----------------------------------- |
| `t`   | Cycle TODO state of current heading |
| `T`   | Insert new TODO heading             |
| `M-t` | Insert new TODO sub-heading         |

## Tables

| 按键绑定      | 说明                                                                       |
|------------- |-------------------------------------------------------------------------- |
| `SPC m t a`   | Align the table at point by aligning all vertical bars                     |
| `SPC m t b`   | Blank the current table field or active region                             |
| `SPC m t c`   | Convert from `org-mode` table to table.el and back                         |
| `SPC m t d c` | Delete a column from the table                                             |
| `SPC m t d r` | Delete the current row or horizontal line from the table                   |
| `SPC m t e`   | Replace the table field value at the cursor by the result of a calculation |
| `SPC m t E`   | Export table to a file, with configurable format                           |
| `SPC m t h`   | Go to the previous field in the table                                      |
| `SPC m t H`   | Move column to the left                                                    |
| `SPC m t i c` | Insert a new column into the table                                         |
| `SPC m t i h` | Insert a horizontal-line below the current line into the table             |
| `SPC m t i H` | Insert a hline and move to the row below that line                         |
| `SPC m t i r` | Insert a new row above the current line into the table                     |
| `SPC m t I`   | Import a file as a table                                                   |
| `SPC m t j`   | Go to the next row (same column) in the current table                      |
| `SPC m t J`   | Move table row down                                                        |
| `SPC m t K`   | Move table row up                                                          |
| `SPC m t l`   | Go to the next field in the current table, creating new lines as needed    |
| `SPC m t L`   | Move column to the right                                                   |
| `SPC m t n`   | Query for a size and insert a table skeleton                               |
| `SPC m t N`   | Use the table.el package to insert a new table                             |
| `SPC m t p`   | Plot the table using org-plot/gnuplot                                      |
| `SPC m t r`   | Recalculate the current table line by applying all stored formulas         |
| `SPC m t s`   | Sort table lines according to the column at point                          |
| `SPC m t t f` | Toggle the formula debugger in tables                                      |
| `SPC m t t o` | Toggle the display of Row/Column numbers in tables                         |
| `SPC m t w`   | Wrap several fields in a column like a paragraph                           |

## Trees

| 按键绑定      | 说明                            |
|------------- |------------------------------- |
| `gj` / `gk`   | Next/previous element (heading) |
| `gh` / `gl`   | Parent/child element (heading)  |
| `gH`          | Root heading                    |
| `ae`          | Element text object             |
| `ar`          | Subtree text object             |
| `M-j` / `M-k` | Move heading                    |
| `M-h` / `M-l` | Promote or demote heading       |
| `M-J` / `M-K` | Move subtree                    |
| `M-H` / `M-L` | Promote or demote subtree       |
| `>>` / `<<`   | Promote or demote heading       |
| `TAB`         | org-cycle                       |
| `SPC m s a`   | Toggle archive tag for subtree  |
| `SPC m s A`   | Archive subtree                 |
| `SPC m s b`   | org-tree-to-indirect-buffer     |
| `SPC m s l`   | org-demote-subtree              |
| `SPC m s h`   | org-promote-subtree             |
| `SPC m s k`   | org-move-subtree-up             |
| `SPC m s j`   | org-move-subtree-down           |
| `SPC m s n`   | org-narrow-to-subtree           |
| `SPC m s N`   | widen narrowed subtree          |
| `SPC m s r`   | org-refile                      |
| `SPC m s s`   | show sparse tree                |
| `SPC m s S`   | sort trees                      |

## Element insertion

| 按键绑定      | 说明                                           |
|------------- |---------------------------------------------- |
| `SPC m i d`   | org-insert-drawer                              |
| `SPC m i D s` | Take screenshot                                |
| `SPC m i D y` | Yank image url                                 |
| `SPC m i e`   | org-set-effort                                 |
| `SPC m i f`   | org-insert-footnote                            |
| `SPC m i H`   | 当前标题或内容之后添加标题 org-insert-heading-after-current |
| `SPC m i h`   | 当前位置插入标题 org-insert-heading            |
| `SPC m i K`   | spacemacs/insert-keybinding-org                |
| `SPC m i l`   | 插入链接 org-insert-link                       |
| `SPC m i n`   | 插入笔记 org-add-note                          |
| `SPC m i p`   | 插入属性 org-set-property                      |
| `SPC m i s`   | 添加父级树 org-insert-subheading               |
| `SPC m i t`   | 添加标签 org-set-tags                          |

## Links

| 按键绑定    | 说明              |
|----------- |----------------- |
| `SPC m x o` | org-open-at-point |

## Babel / Source Blocks

| 按键绑定    | 说明                                     |
|----------- |---------------------------------------- |
| `SPC m b .` | Enter Babel Transient State              |
| `SPC m b a` | org-babel-sha1-hash                      |
| `SPC m b b` | org-babel-execute-buffer                 |
| `SPC m b c` | org-babel-check-src-block                |
| `SPC m b d` | org-babel-demarcate-block                |
| `SPC m b e` | org-babel-execute-maybe                  |
| `SPC m b f` | org-babel-tangle-file                    |
| `SPC m b g` | org-babel-goto-named-src-block           |
| `SPC m b i` | org-babel-lob-ingest                     |
| `SPC m b I` | org-babel-view-src-block-info            |
| `SPC m b j` | org-babel-insert-header-arg              |
| `SPC m b l` | org-babel-load-in-session                |
| `SPC m b n` | org-babel-next-src-block                 |
| `SPC m b o` | org-babel-open-src-block-result          |
| `SPC m b p` | org-babel-previous-src-block             |
| `SPC m b r` | org-babel-goto-named-result              |
| `SPC m b s` | org-babel-execute-subtree                |
| `SPC m b t` | org-babel-tangle                         |
| `SPC m b u` | org-babel-goto-src-block-head            |
| `SPC m b v` | org-babel-expand-src-block               |
| `SPC m b x` | org-babel-do-key-sequence-in-edit-buffer |
| `SPC m b z` | org-babel-switch-to-session              |
| `SPC m b Z` | org-babel-switch-to-session-with-code    |

### Org Babel Transient State

Use `SPC m b .` to enter a transient state for quick source block navigation and  
execution.  During that state, the following bindings are active:  

| 按键绑定 | 说明                          |
|---- |----------------------------- |
| ~'~  | edit source block             |
| `e`  | execute source block          |
| `g`  | jump to named source block    |
| `j`  | jump to next source block     |
| `k`  | jump to previous source block |
| `q`  | leave transient state         |

## 强调 Emphasis

| 按键绑定    | 说明                                                           |
|----------- |-------------------------------------------------------------- |
| `SPC m x b` | **选中文本加粗** make region bold                              |
| `SPC m x c` | `选中文本转换为行内代码` make region code                      |
| `SPC m x i` | *选中文本转换为斜体* make region italic                        |
| `SPC m x r` | 清空选中文本的格式 clear region emphasis                       |
| `SPC m x s` | ~~选中文本添加删除线~~ make region strike-through              |
| `SPC m x u` | <span class="underline">选中文本添加下划线</span> make region underline |
| `SPC m x v` | `选中文本高亮`                                                 |

## 日历中的操作 Navigating in calendar

| 按键绑定 | 说明 |
|----- |---- |
| `M-l` | 右移一天 |
| `M-h` | 左移一天 |
| `M-j` | 上移一天 |
| `M-k` | 下移一天 |
| `M-L` | 右移一月 |
| `M-H` | 左移一月 |
| `M-J` | 上一年 |
| `M-K` | 下一年 |

## Capture buffers and src blocks

`org-capture-mode` and `org-src-mode` both support the confirm and abort  
conventions.  

| 按键绑定                                     | 说明                                   |
|-------------------------------------------- |-------------------------------------- |
| `SPC m <dotspacemacs-major-mode-leader-key>` | confirm in `org-capture-mode`          |
| `SPC m '​`                                    | confirm in `org-src-mode`              |
| `SPC m c`                                    | confirm                                |
| `SPC m a`                                    | abort                                  |
| `SPC m k`                                    | abort                                  |
| `SPC m r`                                    | org-capture-refile in org-capture-mode |

## Org agenda

### Keybindings

The evilified org agenda supports the following bindings:  

| 按键绑定             | 说明                              |
|-------------------- |--------------------------------- |
| `M-SPC` or `s-M-SPC` | org-agenda transient state        |
| `SPC m a`            | org-agenda                        |
| `SPC m C c`          | org-agenda-clock-cancel           |
| `SPC m C i`          | org-agenda-clock-in               |
| `SPC m C o`          | org-agenda-clock-out              |
| `SPC m C p`          | org-pomodoro (if package is used) |
| `SPC m d d`          | org-agenda-deadline               |
| `SPC m d s`          | org-agenda-schedule               |
| `SPC m i e`          | org-agenda-set-effort             |
| `SPC m i p`          | org-agenda-set-property           |
| `SPC m i t`          | org-agenda-set-tags               |
| `SPC m s r`          | org-agenda-refile                 |
| `M-j`                | next item                         |
| `M-k`                | previous item                     |
| `M-h`                | earlier view                      |
| `M-l`                | later view                        |
| `gr`                 | refresh                           |
| `gd`                 | toggle grid                       |
| `C-v`                | change view                       |
| `RET`                | org-agenda-goto                   |
| `M-RET`              | org-agenda-show-and-scroll-up     |

### Org agenda transient state

Use `M-SPC` or `s-M-SPC` in an org agenda buffer to activate its transient state.  
The transient state aims to list the most useful org agenda commands and  
visually organize them by category. The commands associated with each binding  
are listed bellow.  

| Keybinding  | 说明                | Command                           |
|----------- |------------------- |--------------------------------- |
| Entry       |                     |                                   |
| `ht`        | set status          | org-agenda-todo                   |
| `hk`        | kill                | org-agenda-kill                   |
| `hR`        | refile              | org-agenda-refile                 |
| `hA`        | archive             | org-agenda-archive-default        |
| `h:`        | set tags            | org-agenda-set-tags               |
| `hp`        | set priority        | org-agenda-priority               |
| Visit entry |                     |                                   |
| `SPC`       | in other window     | org-agenda-show-and-scroll-up     |
| `TAB`       | & go to location    | org-agenda-goto                   |
| `RET`       | & del other windows | org-agenda-switch-to              |
| `o`         | link                | link-hint-open-link               |
| Filter      |                     |                                   |
| `ft`        | by tag              | org-agenda-filter-by-tag          |
| `fr`        | refine by tag       | org-agenda-filter-by-tag-refine   |
| `fc`        | by category         | org-agenda-filter-by-category     |
| `fh`        | by top headline     | org-agenda-filter-by-top-headline |
| `fx`        | by regexp           | org-agenda-filter-by-regexp       |
| `fd`        | delete all filters  | org-agenda-filter-remove-all      |
| Date        |                     |                                   |
| `ds`        | schedule            | org-agenda-schedule               |
| `dS`        | un-schedule         | org-agenda-schedule               |
| `dd`        | set deadline        | org-agenda-deadline               |
| `dD`        | remove deadline     | org-agenda-deadline               |
| `dt`        | timestamp           | org-agenda-date-prompt            |
| `+`         | do later            | org-agenda-do-date-later          |
| `-`         | do earlier          | org-agenda-do-date-earlier        |
| Toggle      |                     |                                   |
| `tf`        | follow              | org-agenda-follow-mode            |
| `tl`        | log                 | org-agenda-log-mode               |
| `ta`        | archive             | org-agenda-archives-mode          |
| `tr`        | clock report        | org-agenda-clockreport-mode       |
| `td`        | diaries             | org-agenda-toggle-diary           |
| View        |                     |                                   |
| `vd`        | day                 | org-agenda-day-view               |
| `vw`        | week                | org-agenda-week-view              |
| `vt`        | fortnight           | org-agenda-fortnight-view         |
| `vm`        | month               | org-agenda-month-view             |
| `vy`        | year                | org-agenda-year-view              |
| `vn`        | next span           | org-agenda-later                  |
| `vp`        | prev span           | org-agenda-earlier                |
| `vr`        | reset               | org-agenda-reset-view             |
| Clock       |                     |                                   |
| `cI`        | in                  | org-agenda-clock-in               |
| `cO`        | out                 | org-agenda-clock-out              |
| `cq`        | cancel              | org-agenda-clock-cancel           |
| `cj`        | jump                | org-agenda-clock-goto             |
| Other       |                     |                                   |
| `gr`        | reload              | org-agenda-redo                   |
| `.`         | go to today         | org-agenda-goto-today             |
| `gd`        | go to date          | org-agenda-goto-date              |

## Pomodoro

| 按键绑定    | 说明              |
|----------- |----------------- |
| `SPC m C p` | starts a pomodoro |

## Presentation

org-present must be activated explicitly by typing: `SPC SPC org-present`  

| 按键绑定 | 说明           |
|---- |-------------- |
| `h`  | previous slide |
| `l`  | next slide     |
| `q`  | quit           |

## Org-projectile

| 按键绑定          | 说明                                                    |
|----------------- |------------------------------------------------------- |
| `SPC a o p`       | Capture a TODO for the current project                  |
| `SPC u SPC a o p` | Capture a TODO for any given project (choose from list) |
| `SPC p o`         | Go to the TODOs for the current project                 |

## Org-journal

| 按键绑定      | 说明                   |
|------------- |---------------------- |
| `SPC a o j j` | New journal entry      |
| `SPC a o j s` | Search journal entries |

Journal entries are highlighted in the calendar. The following key bindings are  
available for `calendar-mode` for navigating and manipulating the journal.  

| 按键绑定  | 说明                                  |
|--------- |------------------------------------- |
| `SPC m r` | Read journal entry                    |
| `SPC m i` | Insert journal entry for date         |
| `SPC m n` | Next journal entry                    |
| `SPC m p` | Previous journal entry                |
| `SPC m s` | Search all journal entries            |
| `SPC m w` | Search calendar week journal entries  |
| `SPC m m` | Search calendar month journal entries |
| `SPC m y` | Search calendar year journal entries  |

While viewing a journal entry in `org-journal-mode` the following key bindings  
are available.  

| 按键绑定  | 说明                   |
|--------- |---------------------- |
| `SPC m j` | New journal entry      |
| `SPC m p` | Previous journal entry |
| `SPC m n` | Next journal entry     |

## Org-brain

### Application bindings

| 按键绑定    | 说明                         |
|----------- |---------------------------- |
| `SPC a o b` | Visualize an org-brain entry |

### Visualization bindings

| 按键绑定    | 说明                                                                  |
|----------- |--------------------------------------------------------------------- |
| `j / TAB`   | Goto next link                                                        |
| `k / S-TAB` | Goto previous link                                                    |
| `c`         | Add child                                                             |
| `p`         | Add parent                                                            |
| `l`         | Add resource link                                                     |
| `C-y`       | Paste resource link                                                   |
| `a`         | Add resource [attachment](http://orgmode.org/manual/Attachments.html) |
| `o`         | Open and edit the visualized entry                                    |
| `f`         | Find/visit another entry to visualize                                 |
| `r`         | Rename this, or another, entry                                        |
|             |                                                                       |
