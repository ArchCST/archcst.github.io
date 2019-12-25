title: elisp 漫漫长路（二）  
date: 2018-09-20  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# 查询所需要的函数

`M-x apropos`  

# 前缀表达式的一些技巧

```emacs-lisp
    (if test
        (a b)
     (a c)
  ;; 等价于
     (a (if test b c))
```

```emacs-lisp
  (if a a b)
  ;; 可以替换为
  (or a b)
```

```emacs-lisp
  (if a (if b (if c d)
  ;; 则应该是
  (and a b c d)
```

# interactive 交互式命令

交互式命令可以通过 `按键绑定` 或 `M-x` 调用  

`p` 如果有前置参数，将它解释为一个数字，如果没有前置参数，就将参数默认设为1  
`P` 当以交互的方式调用时，将前置参数保持为原始形式（raw form）并将其赋值给参数。通常和 `prefix-numeric-value` 连用  
`b` 只接受已存在的 buffer 名称  

可以使用 C-h f interactive RET 来查看所有支持的字符。  

# 细节

函数名结尾的 `-p` 通常代表 `断言（predicate）` ，返回 `t` 或 `nil` 。因为 elisp 中的判断语句认为 `非 nil (non-nil)` 即可表真，所以有时候为真时并不会返回 `t` 而会返回具体的值。  

例如以下判断会返回 a：  

```emacs-lisp
(let ((var1 "this is non-nil"))
  (if var1 a b)
```

# hook 钩子

钩子是指向某个函数列表的 `变量`  
添加钩子  

```emacs-lisp
(add-hook 'find-file-hooks 'read-only-if-symlink)
```

钩子变量中的函数都不应该设置参数  

移除钩子  

```emacs-lisp
(remove-hook 'find-file-hooks 'read-only-if-symlink)
```

# 匿名函数
