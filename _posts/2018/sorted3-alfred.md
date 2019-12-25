title: 通过 Alfred 从 MacOS 向 Sorted 3 中添加任务  
date: 2018-09-04  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

一直在电脑上用 orgmode，iOS 也下载了 beorg，但总是觉得有一些琐事并不需要使用 orgmode，而且有时候 beorg 的同步会出现一些奇奇怪怪的问题。  

Omnifocus 用过很长一段时间，但既然是作为跟踪琐事来用，Omnifocus 也显得过于庞大了些。  

<!--more-->

Sorted 3 是 iOS 上目前最喜欢的效率软件，目前 Mac 版本还在制作中，暂时可以通过 Alfred 的 Workflow 实现从 Mac 上录入任务到 Sorted。这种方法需要 `Alfred powerpack` 的支持。  

借助 [alfred-reminders](https://github.com/surrealroad/alfred-reminders/blob/master/README.md) 可以实现从 Alfred 录入任务到 Sorted 3，原理是通过 Alfred Workflow 向 Mac OS 的 `提醒事项` 添加任务到一个特定的列表，此列表自动通过 iCloud 同步到 iOS，在下一次打开 Sorted 3 的时候 Sorted 3 会自动读取这个列表，将这个任务最终添加到 Sorted 3 中，并同时将提醒事项中的这个任务标记为完成。听起来很复杂，实际上配置好了后所有步骤都是自动的。  

详细实现步骤如下：  

<!-- more -->

1.  在 iOS 的 `提醒事项.app` 中添加一个列表，你可以给它起名为 `Sorted`
2.  在 iOS 的 `设置` 中点击 `你的名字` → `iCloud` → `提醒事项` 开启右侧的同步开关
3.  打开 `Sorted 3`，点击 `左下角的菜单键` → `设置` → `提醒事项` → 开启右侧的开关，点击 `导入自` ，勾上第 1 步创建的 `Sorted` 列表
4.  点击 [这里](https://github.com/surrealroad/alfred-reminders/releases/download/v74/Reminders.alfredworkflow) 下载 `Alfred Workflow`，并双击添加到 `Alfred` 中
5.  打开 `Alfred`，点击上方的 `Workflows` → 左侧边栏的 `Reminders for Alfred 3`，右侧正中间有个 `>` 符号注明有 "Double-click this to edit default settings"，双击打开
6.  将下方的 `defaultList` 右侧的 `Value` 改为第 1 步创建的 `Sorted` 列表名

然后就可以呼出 `Alfred` 输入 `r 任务标题 回车` 来添加任务了。添加完成后不用打开 `提醒事项` ，直接打开 `Sorted 3` 就能看到该任务已经放入了收件箱。  

Alfred-reminders 也支持一些自然语言，比如 `r tomorrow 开会` ，具体用法可以使用 `r help` 查询或是阅读 [作者的 Github](https://github.com/surrealroad/alfred-reminders/tree/v74)。  
