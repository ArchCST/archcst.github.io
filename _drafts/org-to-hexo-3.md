title: 用 org-mode 写 Hexo 博客（三）：奇淫技巧  
date: 2018-10-12  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# 下划线

输入 `ABC\under{}def` 的时候如果直接输入下划线会 org-mode 当做 `下标` 处理，输出效果会类似这样：ABC<sub>def</sub>  
如果需要输入 `ABC_def` 就必须要用 `~` 或者 `=` 括起来才行。如果想在非行内代码中使用下滑线，可以输入 `\ under{}` （去掉中间空格）即可生成一个不会被当做下标的下划线。  
