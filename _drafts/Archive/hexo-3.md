title: Hexo 静态博客搭建参考（三）：Hexo with spacemacs  
date: 2018-09-4  
updated:  
comments: true  
tags:  

-   learn
-   emacs

categories: learn  
layout: post  
permalink:  

---

开始之前还是先 commit 到版本库避免炸机。本章进一步优化 Hexo with spacemacs 的函数，整体思路是在 Hexo 根目录下建立一个 `raw` 目录，用来存放 `.org` 原文件，然后编写 `elisp` 函数实现快捷键操作，最终目的是解决 Hexo 不支持 orgmode 的痛点。  
因为都是用的 spacemacs 原生配置中的依赖，有这个 15000+ star 的靠山不存在啥时候就不能用了的情况。  
所有代码可以在 [我的 Github](https://github.com/ArchCST/spacemacs) 中找到。这是个大工程，需要不少时间，表中标明了进度，也算是对自己的督促。  
快捷键基于 spacemacs，原生 emacs 需要修改快捷键绑定函数。  

<!-- more -->

{% note info %}  
本文内所有代码能在 [我的 Github](https://github.com/ArchCST/.spacemacs.d) 中找到。  
{% endnote %}  

# 快捷键设计

| 快捷键        | 说明                                                       | 函数名                          | 进度  |
|------------- |---------------------------------------------------------- |------------------------------- |----- |
| `SPC d h e`   | 转换 raw **目录**到 source 的对应目录                      | ArchCST-o2h/export-all-files    | DONE  |
| `SPC d h c`   | 查找不存在对应 org 文件的 md 文件，询问是否移到废篓        | ArchCST-o2h/clean-none-exists   | DONE  |
| `SPC d h h`   | 一键转换、清理                                             | ArchCST-o2h/hyper               |       |
| `SPC d h n`   | 新建 org 草稿                                              | ArchCST-o2h/new-draft           | DONE  |
| `SPC d h p`   | 发布草稿                                                   | ArchCST-o2h/publish-draft       | ISSUE |
| `SPC d h i F` | 插入 [Front-matter](https:--hexo.io-zh-cn-docs-front-matter) | ArchCST-o2h/insert-front-matter | DONE  |
| `SPC d h i m` | 插入"阅读更多"标签                                         | ArchCST-o2h/insert-more         | DONE  |
| `SPC d h i q` | 插入 引用                                                  | ArchCST-o2h/insert-quote        | DONE  |
| `SPC d h i b` | 插入 blockquote                                            | ArchCST-o2h/insert-blockquote   | DONE  |
| `SPC d h i D` | 插入 bs-callout default                                    | ArchCST-o2h/insert-bc-default   | DONE  |
| `SPC d h i P` | 插入 bs-callout primary                                    | ArchCST-o2h/insert-bc-primary   | DONE  |
| `SPC d h i S` | 插入 bs-callout success                                    | ArchCST-o2h/insert-bc-success   | DONE  |
| `SPC d h i I` | 插入 bs-callout info                                       | ArchCST-o2h/insert-bc-info      | DONE  |
| `SPC d h i W` | 插入 bs-callout warning                                    | ArchCST-o2h/insert-bc-warning   | DONE  |
| `SPC d h i -` | 插入 bs-callout danger                                     | ArchCST-o2h/insert-bc-danger    | DONE  |
| `SPC d h i i` | 插入 图片，可填写图片大小                                  | ArchCST-o2h/insert-img-url      | DONE  |
| `SPC d h i f` | 插入 脚注，需要 Hexo 插件支持                              | ArchCST-o2h/insert-footnote     |       |

插入的标签在 [这里](https://hexo.io/zh-cn/docs/tag-plugins#%E5%8F%8D%E5%BC%95%E5%8F%B7%E4%BB%A3%E7%A0%81%E5%9D%97) 能找到说明。  

# 功能说明

## export this file

设定快捷键 `SPC d h e`。  
首先需要在 `./raw` 和 `./source` 两个目录中共同存在 `_posts` 和 `_drafts` 目录，其中 `./raw` 用来存放 `.org` 原文件。  
`SPC d h r` 意为 `=，以下代码实现转换 =raw` 目录中所有文件到对应的 `source` 目录中。  

# 编辑自定义 CSS

Next 6 的 `custom.styl` 文件在 `./themes/next/source/css/_custom/` 目录内，通过修改这个文件可以自定义 CSS  

# 杂项配置

## 标题序号

```shell
npm install hexo-heading-index --save
```

然后在 `站点配置文件` 中添加：  

```yaml
heading_index:
  enable: true
  index_styles: "{1} {1} {1} {1} {1} {1}"
  connector: "."
  global_prefix: ""
  global_suffix: ". "
```

可参考 [Soul Orbit](http://r12f.com/posts/adding-index-to-your-headings-with-hexo-heading-index/) 的配置方法  
