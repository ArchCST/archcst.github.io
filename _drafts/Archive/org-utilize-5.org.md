title: 用好 org-mode 进阶教程（三）：结构设计  
date: 2018-10-15  
updated:  
comments: true  
tags:  

-   learn
-   zen

layout: post  

---

# 截止日期和计划日期

重复需设置在提醒之前  

```sample
 * TODO Pay the rent
   DEADLINE: <2005-10-01 Sat +1m -3d>
```

`C- -1 C-c C-t` 既 Global prefix 为 `-1` 可以彻底完成重复项  

## Repeaters

`+1m` 表示每次完成任务后的下一个月。例如三个月没理发，也没有理由一天理三次发，所以 `+1m` 是完成后立即开始计算下一次开始的表示方法。  
