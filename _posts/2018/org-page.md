title: org-page 安装使用记录  
date: 2018-09-16  
updated:  
comments: true  
tags:  

-   learn
-   emacs

layout: post  

---

org-page 是 [kelvinh](https://github.com/kelvinh/org-page) 制作的一款以 org-mode 文件作为源文件的静态网站生成器，非常的轻量，具有高可定制性。  
我在制作 org-to-hexo 的时候研究很久 org-page 的代码，写得真是漂亮。所以虽然现在不用 org-page 了，依然放上来作为一个参考。  
本文基于 spacemacs 和 shell，用 shell 主要是观察 org-page 具体做了些什么操作。  

<!-- more -->

# spacemacs 下的 org-page 安装和初始化

在 private layer 中申明 org-page 的 layer：  

```shell
(configuration-layer/declare-layers '(
                                      ArchCST-orgpage
                                      ))
```

在 .spacemacs.d/layers/ 中新建同名目录，并添加 packages.el：  

```shell
$ mkdir ~/.spacemacs/layers/ArchCST-orgpage
$ touch ~/.spacemacs/layers/ArchCST-orgpage/packages.el
```

写入到 packages.el：  

```emacs-lisp
(defconst ArchCST-orgpage-packages
  '(org-page))

(defun ArchCST-orgpage/init-org-page ()
  (spacemacs/declare-prefix "ab" "blog")
  (use-package org-page;
    :config (progn (setq op/repository-directory "~/Blog"
                         op/site-main-title "标题"
                         op/site-sub-title "二级标题"
                         op/site-domain "网址"
                         op/personal-github-link "Github链接"
                         ;; op/category-ignore-list '()
                         )
                   (spacemacs/set-leader-keys
                     "abP" 'op/do-publication-and-preview-site
                     "abp" 'op/do-publication
                     "abn" 'op/new-post
                     "abt" 'op/insert-options-template))))
```

分别说明一下绑定的快捷键：  

| 快捷键    | 说明              |
|--------- |----------------- |
| SPC a b P | 生成并上传网页，并提供本地预览 |
| SPC a b p | 直接生成并上传网页 |
| SPC a b n | 新建一片博文，会包含博文的头部信息 |
| SPC a b t | 在当前博文内添加博文头部信息 |

# 初始化 blog 仓库

建立一个目录用来存放 blog 的文件，并初始化 git ：  

```shell
mkdir ~/blog
cd ~/blog
git init
```

在 spacemacs 中执行命令初始化仓库：  

```emacs-lisp
op/new-repository
```

然后输入目录地址：  

```emacs-lisp
~/blog
```

在仓库中 `ls ~/blog` 已经可以看到自动切换到了 source 分支，并默认添加了 index.org about.org README 三个文件。  

在 spacemacs 中打开 `~/blog/index.org` 并使用命令 `SPC a b p` 发布网站，此时再回到 shell 用 `ls ~/blog` 即可看到又自动切换回了 master 分支，并且已经有了 html 文件，静态网站已经生成好了。  

# 建立 Github 远程库存放源文件和网页文件

注意：Github 上新建仓库时 Repository name 写入 `yourname.github.io` 可实现根目录访问，如果 Repository name 写入其他内容则在访问时需要加上`\repository-name`。  

回到 shell，在 master 分支下绑定 origin 远程库并提交：  

```shell
git remote add origin git@github.com:ArchCST/archcst.github.io.git
git push -u origin master
```

这时候就可以通过 `yourname.github.io` 访问你的博客了，初次使用可能需要等待 DNS 20 分钟左右。  

最后再切换回 source 分支，也将其用 origin 管理起来：  

```shell
git checkout source
git push -u origin source
```

此时进入 `https://github.com/ArchCST/yourname.github.io` 可以看到，仓库中一共有两个分支：  

1.  master 分支：是生成的静态网页文件，可以通过 `yourname.github.io` 直接访问网站
2.  source 分支：源文件，里面目前只有 README index.org about.org 三个文件

# org-page 的使用初级使用说明

## 博文的头部信息

使用命令 `SPC a b t` 可以在博文内创建头信息，会创建如下内容：  

```emacs-lisp
#+TITLE:       文章的标题
#+AUTHOR:      作者
#+EMAIL:       邮箱
#+DATE:        日期
#+URI:         统一标识符
#+KEYWORDS:    关键词
#+TAGS:        标签
#+LANGUAGE:    en
#+OPTIONS:     H:3 num:nil toc:nil \n:nil ::t |:t ^:nil -:nil f:t *:t <:t
#+DESCRIPTION: 描述
```

具体的说明和 org-mode 自带的头信息是差不多的，可以参考 [这里](https://orgmode.org/manual/Export-settings.html)   

## 创建 blog 分类，发布博文

在 shell 中进入 blog 目录，并创建一个 blog 子目录，用来存放 .org 源文件：  

```shell
cd ~/blog
mkdir blog
cd blog
touch firstblog.org
```

让 org-page 自动生成 master 分支的 html 文件之前，最好手动 push source 分支，避免出现问题新建目录无法识别等问题，只要辛苦码的内容放进了版本库，怎么乱来都放心。  

顺便添加 `.gitignore` 文件  

```shell
cd ~/blog
vim .gitignore
git add .
git commit -m 'add firstblog.org'
git push
```

然后在 spacemacs 中打开刚刚创建的 firstblog.org，使用命令 `SPC a b t` 创建文件头信息。  

随便输入一些正文后，就可以使用 `SPC a b p` 发布到 `yourname.github.io` 中了，非常的方便。  

使用 `SPC a b p` 时询问四次，分别是：  

1.  是否发布所有 .org 文件
2.  是否发布到一个文件夹预览
3.  是否自动 commit
4.  是否自动 push 到远程库

一般我会使用 `y n y y`，直到出现 `publication finished` 即表示发布成功。  

## 主页和关于

默认建立的 `index.org` 和 `about.org` 中既是主页和关于页面的内容，当然你也可以自己添加独立页面。  

编辑这两个文件即可。  

# 以后的使用

所以总结一下，以后发布博文的操作步骤如下：  

1.  确认仓库是在 source 分支内
2.  复制或者创建一个 .org 文件到 blog 目录中
3.  `SPC a b t` 为此博文创建头信息
4.  `git add` 和 `git commit` 将改动保存到版本库，若有需要可以再 `git push` 把 source 分支同步到远程库
5.  `SPC a b p` 发布博客
