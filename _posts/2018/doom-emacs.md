title: 终于对 Spacemacs 忍无可忍，你好 Doom-Emacs  
date: 2019-07-22  
updated:  
comments: true  
tags:  

-   cheatsheet

layout: post  

---


# 配置

添加环境变量  

```shell
# 在~/.zshrc 最后一行添加
export PATH=$PATH:~/.emacs.d/bin
# 之后就可以使用这些代码了
doom help # 查看帮助
doom refresh # 安装删除包
```

<!--more-->

还是使用了 `fcitx.el` 来避免跳出 insert-state 时仍旧是是中文输入法  

```emacs
(setq fcitx-active-evil-states '(insert emacs hybrid))
  (fcitx-aggressive-setup)
  (fcitx-prefix-keys-add "M-x")
  (fcitx-prefix-keys-add "SPC")
```


# 快捷键


## 常用快捷键

| 快捷键    | 说明               |
|--------- |------------------ |
| `SPC .`   | 打开文件           |
| `SPC f r` | 打开最近打开过的文件 |
| `SPC b B` | 在所有已打开的 Buffer 中切换 |
| `u`       | 撤销               |
| `C-r`     | 重做               |
| `C-x u`   | undo-tree，可视化撤销和重做 |
| `g c c`   | 注释               |
|           | 代码对齐           |
|           | 80格子             |


## 工作区 Workspace

使用工作区可以保存当前项目的编辑状态，这样可以快速进入工作状态。工作区和  
projectile 配合使用是非常舒服的。  

从 Doom 的快捷键设计可以看出，Doom 的 workflow 基本上就是围绕着工作区展开的。  

使用 `SPC TAB n` 新建工作区，建立好之后 `SPC TAB r` 对当前工作区命名，进入到工作  
状态后 `SPC TAB s` 进行保存。之后就可以通过 `SPC TAB .` 来切换工作区了。同时也可  
以通过 `SPC TAB 数字` 来快速切换。  

当不再需要某个工作区时，可以通过 `SPC TAB d` 来删除当前工作区。  

重启 emacs 后需要使用 `SPC TAB l` 来加载工作区。  

工作区1名字为 `main` 应该留作处理一切非项目的工作，例如 `org-mode` 等。  

常用的快捷键：  

| 快捷键    | 说明               |
|--------- |------------------ |
| `SPC ,`   | 切换到当前项目已经打开的buffer |
| `SPC SPC` | 切换到当前项目中的任意文件 |


## 搜索

| 快捷键    | 说明                                  |
|--------- |------------------------------------- |
| `SPC / p` | 在整个项目的所有文件中搜索            |
| `/`       | 在当前 buffer 中向下搜索，使用 `n` 或者 `N` 在结果中跳转 |
| `?`       | 在当前 buffer 中向上搜索，使用 `n` 或者 `N` 在结果中跳转 |

使用 `SPC / p` 在整个项目中搜索关键字，之后可以通过 `C-c C-e` 在搜索结果中进行编  
辑。编辑完成后通过 `C-c C-c` 提交，或者通过 `C-c C-k` 放弃更改。  


## 光标快速移动

例句：This is not Iris, this is Isabel  
This is not Iris, this is Isabel  
把光标放到这一句的开头，在 normal-state 下按下 `s` 启动 [evil-snipe](https://github.com/hlissner/evil-snipe) ( `S` 向前)，这时输入  
`is` 可以将光标移动到下一个匹配 `is` 的位置，同时标注这一句话中所有的 `is` ，不  
区分大小写，然后可以使用 `;/,` 向后/向前移动光标。  

另外一个在可见区域内移动光标的方式是使用 [avy](https://github.com/abo-abo/avy) ，快捷键是 `g s SPC` 。按下快捷键后  
输入目标文字，稍等0.5秒会出现对应的字母，按下对应字母就可以将光标移动过去。  

这两个光标移动的方式可以极大的提高编辑速度。  

另外 avy 还提供了一个 `avy-goto-char-2` 的函数，输入两个字符移动到匹配位置，也非  
常好用。  

无论是 evil-snipe 还是 avy 都可以使用 `v` 来快速选中，区别是 `s` 会包含定位字  
符， `g s SPC` 则不会。  


## 多光标

evil-multiedit 方式  
使用 `v` 选中文本后，键入 `R` 可以进入多光标编辑模式，会自动选中 buffer 中其他匹  
配的文本。modeline最左边也将现实匹配的文本数量。  

这时使用 `C-n` `C-p` 可以在匹配文本中轮换，按 `RET` 反选当前匹配项。选好候选项后  
编辑其中一个匹配项，再按 `ESC` 退出即可应用到所有选中的匹配项。  

如果不想全部选中，那么 `v` 选中文本后使用 `M-d` `M-D` 则可以向上向下一条一条的匹  
配。  

evil-mc 方式  
那么如何在任意地方插入光标呢，使用 `g z z` 在当前位置创建光标即可。还有一些方法  
如下：  

| 快捷键  | 说明                |
|------- |------------------- |
| `g z m` | 标记当前单词的所有匹配项 |
| `g z d` | 标记当前单词并移动到下一个匹配项 |
| `g z j` | 在当前位置创建光标，将光标移动到下一行 |
| `g z u` | 取消上一个光标，类似于 undo |


# 遗留问题

`SPC SPC` 中已经删除的文件依然存在  


# 参考资料

[Getting started with Doom Emacs](https://medium.com/@aria_39488/getting-started-with-doom-emacs-a-great-transition-from-vim-to-emacs-9bab8e0d8458)  
[Noel Welsh: Doom Emacs Workflows](https://noelwelsh.com/posts/2019-01-10-doom-emacs.html)
