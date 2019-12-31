title: 折腾 Centaur Emacs 基本配置  
date: 2019-01-01  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# 为什么用 Centaur Emacs

用了一年 Spacemacs 后，感觉有点臃肿。特别是在完全体验过 vim、emacs、hybrid 三种模式后，还是更喜欢纯粹的 emacs 模式。  
虽然 vim 确实在文本编辑上更胜一筹，但 Steve purcell 也在 [这里](https://www.sanityinc.com/articles/vim-vs-emacs/) 写明了为什么他不用 evil，深以为然。于是考虑更换为更轻量级的原生 emacs 体验的配置文件。  
正好发现了 [Centaur Emacs](https://github.com/seagle0128/.emacs.d) 这个国人的配置，对 Pinyin search 功能特别有兴趣，也满足了 UI 漂亮，配置文件一目了然的需求，便萌生了折腾一番的冲动。  

# 预期目标

## 创建并维护自己的目录

Centaur 为用户预留的 custom 文件为 `~/.emacs.d/custom-post.el` 这个文件默认是不存在的，需要手动建立。  

首先需要把自己建立的目录放到 `load path` 中，代码如下：  

```emacs-lisp
;;; custom-post.el --- Personal setup file    -*- no-byte-compile: t -*-
;;; Commentary:
;;;       This is my personal setup
;;; Code:

;; set personal load path
(add-to-list 'load-path "/Users/cst/.emacs.d/ArchCST/")

;; Load my .el files in ~/.emacs.d/ArchCST/
(require 'ArchCST-settings)
(require 'ArchCST-org)
(require 'ArchCST-keybindings)

(provide 'custom-post)
;;; custom-post.el ends here
```

这样可以把所有自定义的 `.el` 文件放到 `ArchCST` 目录中了，首先我在 `ArchCST` 目录内建立了三个 `.el` 文件：  

-   `ArchCST-settings.el` 用来写一些基本的配置
-   `ArchCST-org.el` 用来写 org-mode 的配置
-   `ArchCST-keybindings.el` 用来写所有的自定义按键绑定

## 解决中文的一些相关问题

### 字体

选定使用 [更纱黑体](https://github.com/be5invis/Sarasa-Gothic) ，这是个等宽字体，并且英文字母进行了宽度调整，非常适合在 Emacs 中使用，写代码感觉也非常好。  
我的 `~/.emacs.d/ArchCST/ArchCST-settings.el` 文件内容如下，其中包含了字体的设置：  

```emacs-lisp
;;; ArcsCST-settings.el --- default settings    -*- no-byte-compile: t -*-
;;; Commentary:
;;;       This is my personal setup
;;; Code:

;; ^L show as a line，use C-q C-l to add a page-break
(global-page-break-lines-mode t)

;; Default font
(add-to-list 'default-frame-alist '(font . "Sarasa Mono SC-16"))

(provide 'ArchCST-settings)
;;; ArchCST-settings ends here
```

注意： `Sarasa Mono SC` 是字体的名字， `16` 是字号。另外就是字号需要设置为偶数，奇数字号中英文对齐会有问题。  

### 输入法切换

最新的 MacOS/iOS 均内置了小鹤双拼，终于可以扔掉厌恶已久的某犬类驰名输入法。再加上 HHKB 的键位设计，有这么一个想法：  
左下角的 Opt 键单独按下时切换输入法，这样可以用手掌按到，切换非常方便。而和其他键组合使用的时候，作为 `super` 键使用。同时增加一个 previous-buffer 的快捷键： `Opt+TAB` 。  
为了实现这个功能，需要使用到 MacOS 下的改建神器：[Karabiner-Elements](https://pqrs.org/osx/karabiner/) （简称 KE）  

新建 `~/.config/karabiner/assets/complex_modifications/left-option-key.json` ，内容如下：  

```json
{
    "title": "Change left_option key",
    "rules": [
        {
            "description": "Change left_option to F19 if pressed alone.",
            "manipulators": [
                {
                    "type": "basic",
                    "from": {
                        "key_code": "left_option",
                        "modifiers": {
                            "optional": [
                                "any"
                            ]
                        }
                    },
                    "to": [
                        {
                            "key_code": "left_option"
                        }
                    ],
                    "to_if_alone": [
                        {
                            "key_code": "f19"
                        }
                    ]
                }
            ]
        }
    ]
}
```

这段代码表示左 Opt 键单独按下时作为 F19 键，和其他按键组合使用的时候依旧作为 Opt 键使用。  
保存了之后在 KE 的 `Complex Modifications` 中点击左下角的 `Add Rule` 便能找到 `Change left_option to F19 if pressed alone.` 点击 `Enable` 即可启用。  
然后在 MacOS 的 `系统偏好设置 → 键盘 → 快捷键 → 输入法` 中设置切换输入法的快捷键为 `F19` 便完成了设置。  

这段代码修改一下 `key_code` 可以作用于任何修饰键，具体的 `key_code` 可以在 [这里](https://github.com/tekezo/Karabiner-Elements/issues/925) 找到。  

在 `ArchCST-keybindings` 中，我设置了 Command 作为 Meta 键，Option 作为 Super 键。大写锁定键直接用 HHKB 跳线作为了 Control，不需要管它。代码如下：  

```emacs-lisp
;;; ArchCST-keybindings.el --- keybindings

;;; Commentary:
;;        My personal keyindings

;;; Code:

;; 这里控制了 command 和 option 两个按键对应的功能键
(setq mac-command-modifier 'meta)
(setq mac-option-modifier 'super)

;; 中文输入状态下，很多时候会需要使用半角的 ~ 而不是全角的 ～
;; 这里使用 super+· （中文输入法下的 ` 键） 来快速输入 ~
(global-set-key (kbd "s-·") "~")

;; 默认的 previous-buffer 会在所有已打开的 buffers 中切换，所以定义了一个函数来只在最近访问过的两个 buffers 之间切换
(global-set-key (kbd "s-<tab>") 'ArchCST-previous-buffer)

(provide 'ArchCST-keybindings)
;;; ArchCST-keybindings ends here
```

这个 `ArchCST-previous-buffer` 方法在 [这里](https://emacsredux.com/blog/2013/04/28/switch-to-previous-buffer/) 可以找到，我是放在了 `ArchCST-funcs.el` 中。  

### 中文输入法下的命令输入（MacOS）

在中文输入法下输入命令（例如 C-x b）那么这个 `b` 会被输入法劫持，作为中文输入的一部分，需要再次按下 RET 把这个 `b` 作为英文字符传入 emacs。这是我们不想看到的。  
之前搞 spacemacs 的时候是通过 [fcitx.el](https://github.com/cute-jumper/fcitx.el) 来实现自动切换输入法的，这个 package 也支持纯 emacs 下使用，应该说在纯 emacs 快捷键下支持得更好一些。  

第一步，克隆 [fcitx-remote-for-osx](https://github.com/CodeFalling/fcitx-remote-for-osx) ，然后根据输入法识别名称编译，具体可以看我的 [另一篇博客](https://archcst.me/201809/loginput-remote-osx.html) 。测试通过之后便可以通过 fcitx-remote-for-osx 来控制输入法的切换了。  

第二步，使用 [fcitx.el](https://github.com/cute-jumper/fcitx.el) 来控制在 mini-buffer 和某些前缀快捷键按下时启用英文输入法。  

执行 M-x package-install RET fcitx RET 安装 fcitx.el，然后在配置文件中添加：  

```emacs-lisp
;; fcitx.el settings
(require 'fcitx)

;; 如果你基本不在 mini-buffer 中使用中文输入法，那么使用 aggressive-setup 将会直接在 mini-buffer 中禁用中文。
(fcitx-aggressive-setup)

;; Emacs 中 C-s 默认调用的检索工具为 I-search ，如果希望在 I-search 中不切换为英文，可以添加这行代码
;; (fcitx-isearch-turn-on)  ;; Centaur Emacs 使用 Pinyin-search 和 swiper，并不需要这一行

;; 可以通过这个 list 来定义哪些按键之后切换为英文输入，可以添加自定义的快捷键前缀，比如我的个人常用快捷键集合 C-i
(fcitx-prefix-keys-add "C-x" "C-c" "C-h" "M-s" "M-o" "C-i")

;; 设置完成后启用 prefix-keys
(fcitx-prefix-keys-turn-on)
```

然后重启 emacs 之后测试一下 M-x 等快捷键，会发现自动切换为英文了。C-x b 的时候 b 也不会再被中文输入法拦截。  

### Org mode 中文自动换行

M-q 在 Org mode 中并不能很好地起到换行的作用。  
可以使用 refill-mode 来自动换行，但这有个问题，导出的时候会把断行视为一个空格， [这里](https://github.com/hick/emacs-chinese) 有个解决方案。  

## 排除 recentf 中的 agenda-files

如果使用过 agenda-view，recentf（C-x C-r）中会出现所有在 agenda-files 列表中的 org-mode 文件。  
然而一般情况下我们找 agenda-files 的文件是通过 agenda-view ，所以需要将 agenda-files 排除掉，[这里](https://github.com/rakanalh/emacs-dashboard/issues/51) 有个方法，原理是把 agenda-files 添加到 recentf-exclude 列表中：  

```emacs-lisp
(setq recentf-exclude (-map 'f-canonical (org-agenda-files))
```

之后再执行一下 `M-x recentf-cleanup` 就好了。  

## 学习英语

使用 LazyCat 的 [company-english-helper](https://github.com/manateelazycat/company-english-helper) ，Readme 已经写得很详细了， `M-x toggle-company-english-helper` 就可以愉快的使用。  

# 快捷键，以及一些 Tips

## 基本快捷键

[这里](https://aifreedom.com/technology/112) 有一份比较详尽的默认快捷键列表  

| 快捷键     | 说明               |
|---------- |------------------ |
| C-h m      | 查看当前主模式的说明文档 |
| M-q        | 当前段落对齐 C-x f 设定的宽度 |
| C-x f      | 设定文本宽度       |
| C-s        | 搜索               |
| C-r        | 反向搜索           |
| C-c y      | 有道查词           |
| C-q C-l    | 添加分隔符         |
|            | 删除文件           |
|            | 重命名文件         |
| C-x u      | 可视化 undo-tree   |
| dired-mode |                    |
|            |                    |

## 自定义的快捷键

| 快捷键  | 说明                  |
|------- |--------------------- |
| s-·     | 输入半角 "~" 符号     |
| s-<tab> | 在最近访问的两个 buffers 之间切换 |
| s-s     | 项目内查找            |
| s-p     |                       |

## 窗口（Frame）相关的快捷键

| 快捷键 | 说明         |
| C-x 1 | 关闭其他窗口 |
| C-x o | 切换到另一个 Frame |
| C-x 2 | 纵向分割窗口 |
| C-x 0 | 关闭当前 Frame |
| C-M-v | 另一个窗口向下翻页 |
| C-M-V | 另一个窗口向上翻页 |
