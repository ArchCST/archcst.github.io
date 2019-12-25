title: Hexo 静态博客搭建参考（二）：使用 org-mode 编写博文  
date: 2018-09-02  
updated: 2018-09-04  
comments: true  
tags:  

-   learn
-   emacs

layout: post  

---

上一章已经使用 Github 开始管理整个博客目录，在每一次对 Hexo 进行配置之前记得先 commit，以免炸机。  
用 emacs 时间长了对所有其他轻量级标记语言都熟视无睹，只有 [Org mode](https://orgmode.org/) 才是真爱。本章实现用 `.org` 文件写博客，如果你用 Markdown，请跳过这一章，Hexo 默认的语法就是 Markdown。  
[hexo-renderer-org](https://github.com/coldnew/hexo-renderer-org) 已经很久不更新，目前是无法使用的状态，经过一下午的 Debug 并没有找到好的修复方法。目前**唯一**的方法是将 `.org` 文件转换为 Hexo 支持的 GitHub Flavored Markdown (GFM) 格式。  
本来以为半小时可以搞定，结果被 `metadata` 问题卡了一整天…查阅了无数文档翻遍了 Github 终于还是搞定了，就当练英语了。  
用 emacs 的人还是太少了啊。  

<!-- more -->

# 配置 ox-gfm

emacs 原生的 `ox-md.el` 支持的是 2004 年版本标准的 Markdown 格式，需要使用 [ox-gfm](https://github.com/larstvei/ox-gfm) 来实现对 GFM 的支持  
如果是原生 emacs，在配置文件中添加 `require`：  

```emacs-lisp
(eval-after-load "org"
  '(require 'ox-gfm nil t))
```

我用 [spacemacs](http://spacemacs.org/)，自带了 `ox-gfm`，默认没有启用，需要在 `org layer` 中开启  
在 `~/.spacemacs` 或 `~/.spacemacs.d/init.el` 中找到 `dotspacemacs-configuration-layers`：  

```emacs-lisp
(org :variables
      org-enable-github-support t)
```

这样就开启了 `ox-gfm`，通过 `ox-gfm` 可以实现 .org 文件对 GFM 文件的转换  

## 让 ox-gfm 更好的适配 hexo

在 `private layer` 中添加代码，我是直接写在 `~/.spacemacs.d/layers/ArchCST/funcs.el`中的：  

```emacs-lisp
;; transfer .org file to .md format for Hexo
(defun ArchCST/hexo-ox-gfm (&optional async subtreep visible-only)
  (interactive)
  (let ((outfile (org-export-output-file-name ".md" subtreep "~/git/CSTHexo/source/_posts")))
    (org-export-to-file 'gfm outfile async subtreep visible-only)))

(spacemacs/set-leader-keys "dhe" 'ArchCST/hexo-ox-gfm)
```

这段代码利用 GFM 作为 backend 来输出 Markdown 文件，文件名保持和源文件一致，然后自动输出到 Hexo 目录中。  
第三行末尾的路径改为你自己的 `_posts` 目录路径，最后一行我绑定了快捷键 `SPC d h e`，如果是原生 emacs 可根据自己需要写 `global-set-key` 绑定快捷键  

# Metadata 的处理

Hexo 接受以下 Metadata：  

```yaml
layout: post
title: 标题
date: 2018-09-1
updated: 2019-09-02
comments: true
tags:
  - tag1
  - tag2
categories: 分类
permalink: 覆盖文章网址
------
```

`yaml` 的 `metadata` 以 `---` 结尾，但经过 `ox-gfm` 转义后 `---` 会转换为分页符，解决方法是用 6 个短横线 `------` 代替。  
还需要添加 `org mode` 的 metadata：`#+OPTIONS: toc:nil \n:t`，以免生成 `TOC (table of content)` 导致 Hexo 无法读取 `yaml` 配置  
其中 `toc:nil` 表示不生成 Markdown 文件内的 `TOC`，否则会干扰 Hexo 读取 yaml。我们使用 Next 主题的 `TOC`，更美观实用  
同时根据 `.org` 文件的默认设置，文本之间需要空一行，否则转换之后多行之间将没有换行符。添加 `\n:t` 即可实现自动给提行添加换行符  
合在一起头部信息应该像这样：  

```yaml
#+OPTIONS: toc:nil \n:t
layout: post
title: 标题
date: 2018-09-1
updated: 2019-09-02
comments: true
tags:
  - tag1
  - tag2
categories: 分类
permalink: 覆盖文章网址
------
```

这里也提供一段自己写的自动添加头部信息的代码，实现了用快捷键 `SPC d h i` 自动在文件头部添加 [Hexo 文档](https://hexo.io/zh-cn/docs/front-matter) 提到的所有 metadata，然后自动跳转到 `title:` 尾部进入 insert mode  
已经预留了 yaml 所必须的空格，使用上非常方便。  

```emacs-lisp
;; insert metadata at the top of file for Hexo
(defun ArchCST/hexo-insert-metadata ()
  (interactive)
  (evil-goto-first-line)
  (insert-before-markers "#+OPTIONS: toc:nil \\n:t
title: 
date: 
updated: 
comments: true
tags:
  - 
categories: 
layout: post
permalink: 
------
")
  (evil-goto-line 2)
  (evil-append-line 0)
  )

(spacemacs/set-leader-keys "dhi" 'ArchCST/hexo-insert-metadata)
```

这段代码针对 Spacemacs 的 Vim 模式用户，用原生 emacs 配 evil 也可修改末行的快捷键绑定函数来使用。用 emacs 原生编辑模式可能需要删掉 `evil` 相关行。  

`data:` 和 `updated:` 后面可以使用 `SPC m d t` 或者 `C-c .` 来添加 Org mode 的时间戳，不必使用手动录入。  

# 支持的标签

代码框内的为 `.org` 文件中的原文，代码框之后的是效果。  
另外，代码块依旧是用 `#+BEGIN_SRC language` 和 `#+END_SRC` 包起来  

## 字体

.org 文件中的原文：  

```sample
中间的/斜体/为测试文本
中间的*粗体*为测试文本
中间的~代码~为测试文本
中间的=代码=为测试文本
中间的+删除+为测试文本
```

效果：  
中间的*斜体*为测试文本  
中间的**粗体**为测试文本  
中间的`代码`为测试文本  
中间的`代码`为测试文本  
中间的~~删除~~为测试文本  

## 引用

```sample
{% cq %} 
/斜体/ *粗体* ~代码~ =代码= +删除+
{% endcq %}
```

{% cq %}  
*斜体* **粗体** `代码` `代码` ~~删除~~  
{% endcq %}  

## Bootstrap Callout

```sample
{% note default %} 
default /斜体/ *粗体* ~代码~ =代码= +删除+
{% endnote %}
{% note primary %} primary {% endnote %}
{% note success %} success {% endnote %}
{% note info %} info {% endnote %}
{% note warning %} warning {% endnote %}
{% note danger %} danger  {% endnote %}
```

{% note default %}  
default *斜体* **粗体** `代码` `代码` ~~删除~~  
{% endnote %}  
{% note primary %} primary {% endnote %}  
{% note success %} success {% endnote %}  
{% note info %} info {% endnote %}  
{% note warning %} warning {% endnote %}  
{% note danger %} danger  {% endnote %}  

主题配置文件中的 `# Note tag (bs-callout)` 段落可以对 Bootstrap Callout 进行样式上的配置。  

[标签插件（Tag Plugins） | Hexo](https://hexo.io/zh-cn/docs/tag-plugins) 中有更多的内建标签用法  

# 遗留问题（Solved）

本文提供的代码可以在 [我的 Github](https://github.com/ArchCST/spacemacs) 上找到。  

本章实现的方式基本上解决了 orgmode with hexo 的诸多问题，实际上是使用了比较苯的方法，只是在使用上还算方便。  

目前遗留的问题是 `<!-- more -->` 标签阅读更多这一块还没有找到合适的解决方案，暂时只能用 Next 主题配置文件提供的：  

```yaml
auto_excerpt:
  enable: true
  length: 300
```

来实现以固定字数自动生成摘要，无法精确控制。  

如果你有解决办法，或者更好的方式实现 Orgmode with Hexo，请在本文下面留言、[Telegram](http://t.me/archcst)，或者在 [我的 Github](https://github.com/ArchCST/spacemacs) 中提交 issue，万分感谢！  

## 解决方案

可以通过添加 `#+HTML: <!-- more -->` 来解决，ox-gfm 会跳过所有的 `#+HTML:` 行。  

同时，没有特殊字符的 `html 标签` 也是不会被转义的，基本上都可以直接使用，这就给自定义 <span class="cst-red">CSS</span> 提供了土壤…  
