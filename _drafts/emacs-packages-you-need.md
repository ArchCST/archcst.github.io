- [projectile](#sec-1)
- [Treemacs](#sec-2)
- [dired-mode](#sec-3)
- [YASnippet](#sec-4)

# projectile<a id="sec-1"></a>

[官方文档](https://docs.projectile.mx/en/latest/usage/)

项目管理，只需要在项目的根目录添加 `.projectile` 然后在 emacs 中打开任意一个该项目目录内的文件，就可以在 `C-c p p` 中找到这个项目。 我已经把 `s-p` 绑定到了 `counsel-projectile-switch-project` 上了，需要切换项目的时候非常方便。

# Treemacs<a id="sec-2"></a>

[官方 Github](https://github.com/Alexander-Miller/treemacs)

侧边栏文件目录树。文档巨长，最常用的如下：

| 快捷键 | 说明             |
| ?   | Treemacs 中的快捷键说明 |
| n   | 下一个文件       |
| p   | 上一个文件       |
| u   | 父目录           |
| TAB | 展开             |
| M-n | 上一个同级       |
| M-p | 下一个同级       |
| c f | 新建文件         |
| c p | 新建目录         |

# dired-mode<a id="sec-3"></a>

默认的文件管理器，主要快捷键为

# YASnippet<a id="sec-4"></a>

[官方 Github](https://github.com/joaotavora/yasnippet) ，[官方文档](http://joaotavora.github.io/yasnippet/) ， [这里](https://github.com/lujun9972/emacs-document/blob/master/emacs-common/%E5%9C%A8Spacemacs%E4%B8%AD%E4%B8%BAYasnippet%E6%B7%BB%E5%8A%A0%E8%87%AA%E5%AE%9A%E4%B9%89snippet.org) 还有一份不错的教程，虽然是 spacemacs 下的教程，但写得很全面。不用 spacemacs 的话目录不是 `.emacs.d/private/snippets` ，而是 `.emacs.d/snippets` 。 [这里](https://github.com/AndreaCrotti/yasnippet-snippets) 有一个官方的 Snippets 的集合，根据不同的 mode 归了类， `M-x yas-describe-tables` 可以看到当前 mode 提供的 snippets。

基本快捷键：

| 快捷键    | 说明            |
| TAB       | 补全            |
| C-c & C-n | 创建新的 snippet |
| C-c & C-v | 查看和修改某个 snippet |

基本上就可以使用了。值得注意的是，使用 C-SPC 选中文本之后按下 `C-c & C-n` 可以根据选中的文本创建 snippet，非常方便。

例如每次新建一个 `.el` 文件的时候都需要输入版权信息等，所以我建立了一个 snippet，内容如下：

```emacs-lisp
# -*- mode: snippet -*-
# name: new-el-file
# key: nel
# --
;;; `(file-name-nondirectory (buffer-file-name))` --- $1	-*- lexical-binding: t -*-

;; Copyright (C) 2019 ArchCST

;; Author: ArchCST <cst@crystl.cc>
;; URL: https://ArchCST.me

;; This file is not part of GNU Emacs.
;;
;; This program is free software; you can redistribute it and/or
;; modify it under the terms of the GNU General Public License as
;; published by the Free Software Foundation; either version 2, or
;; (at your option) any later version.
;;
;; This program is distributed in the hope that it will be useful,
;; but WITHOUT ANY WARRANTY; without even the implied warranty of
;; MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
;; General Public License for more details.
;;
;; You should have received a copy of the GNU General Public License
;; along with this program; see the file COPYING.  If not, write to
;; the Free Software Foundation, Inc., 51 Franklin Street, Fifth
;; Floor, Boston, MA 02110-1301, USA.
;;

;;; Commentary:
;;
;;      $2
;;

;;; Code:

$0

(provide '`(file-name-nondirectory (file-name-sans-extension (buffer-file-name)))`)
;;; `(file-name-nondirectory  (buffer-file-name))` ends here
```

其中可以看到有三个地方有 elisp 代码，这三段代码都是插入当前文件名的，避免手动输入。 elisp 代码应该用 `` `` `` 括起来。嵌入 elisp 代码可以完成很多事情，比如自动填入当前时间等等。有了嵌入的 elisp 你的脑洞大小便是唯一的限制了。 `$1` `$2` 等则是按下 TAB 之后光标的跳转位置。 `$0` 比较特殊，序号虽然是 0，但是会最后一个跳转到那里，同时再按 TAB 就不会再跳转了，一般是放在会最后编辑的部分。 这个文件放入到 `.emacs.d/snippets/emacs-lisp-mode/` 中之后，在任意 emacs-lisp-mode 的 buffer 中输入 `nel` 然后按下 TAB 键便可以插入这个 snippet。
