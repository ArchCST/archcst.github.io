title: Hexo 静态博客搭建参考（一）：本地搭建  
date: 2018-09-01  
updated: 2018-09-04  
comments: true  
tags:  

-   learn
-   emacs

layout: post  

---

Mac 使用 Hexo 搭建个人博客，文中但凡用到相对路径 `./` 均代表博客的根目录。  
官方文档：[文档 | Hexo](https://hexo.io/zh-cn/docs/)  

# 准备工作

Mac OS 下安装 `git` `node.js` `hexo`  

```shell
brew install git
brew install node
sudo npm i -g npm
sudo npm install -g hexo-cli
```

<!-- more -->

# 本地搭建

我在 Mac 中 `~/git` 目录存放所有我用 `git` 管理的项目，Hexo 也打算用 git 管理起来，所以依旧存放在 `~/git` 目录中，命名为 `CSTHexo`。  

建立目录并初始化 Hexo：  

```shell
mkdir ~/git/CSTHexo && cd ~/git/CSTHexo
hexo init
npm install
```

在本地搭建就这么简单，已经完成了。继续输入 `hexo s` 即可启动本地预览，然后在浏览器中进入 `localhost:4000` 就能看到已搭建好的本地博客。  
可以看到目前页面中已经有一篇 `Hello Word` 的博文，已经比较详细的说明了 Hexo 的基础使用方法，更多命令在 [这里](https://hexo.io/zh-cn/docs/commands)  

# 用 Github 管理 Hexo

我只是用 Git 管理 Hexo，并不打算发布到 Github pages 中。  
先在 Github 上建立好仓库，然后本地初始化并关联：  

```shell
cd ~/git/CSTHexo
git init
git add *
git commit -m 'first commit'
git remote add origin git@github.com:ArchCST/CSTHexo.git
git push -u origin master
```

因为 Hexo 已经为你写好了 `.gitignore` 所以直接 `git add *` 即可，不需要自己去写。  

# Hexo 基础配置

Hexo 的目录结构：  

```shell
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

配置文件 \_config.yml，字段描述在 [这里](https://hexo.io/zh-cn/docs/configuration)。注意 yaml 文件的字段和值之间必然有一个空格。  

```shell
title: ArchCST's Blog # 正确
title:ArchCST's Blog  # 错误
```

## 永久链接

[永久链接](https://hexo.io/zh-cn/docs/permalinks) 中解释了如何给每一篇博文配置它的永久链接，我的配置如下：  

```yaml
:year:month/:title.html
```

实现的效果为当某一篇文件名为 `hexo-1.md` 博文的 [Front-matter](https://hexo.io/zh-cn/docs/front-matter) 如下时：  

```markdown
title: Hexo 静态博客搭建参考（一）：本地基础搭建
date: 2018-09-01 12:00:00
```

永久链接为：  

```html
201809/hexo-1.html
```

## Asset 文件夹

[资源文件夹](https://hexo.io/zh-cn/docs/asset-folders) 可以更方便和集中的管理非博文资料，其实主要就是图片。开启 `post_asset_folder: true` 即可。  

# Next 主题

选用的 [Next](https://github.com/theme-next/hexo-theme-next) 主题。原因很简单， [文档](https://github.com/theme-next/hexo-theme-next/tree/master/docs) 写得好。  
根据 [安装说明](https://github.com/theme-next/hexo-theme-next/blob/master/docs/INSTALLATION.md) 安装最新的 release，也非常简单：  

```shell
cd ~/git/CSTHexo
mkdir themes/next
curl -s https://api.github.com/repos/theme-next/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1
```

之所以这么安装而没有使用克隆的方式是没有必要使用 submodule。  

然后在 \_config.yml 最下面使用 `theme: next` 即可启用主题。  

## 主题的 data files 配置

为了避免修改默认的 `next/_config.yml` 可以通过使用 Hexo 的 `data files` 功能，这样在升级主题的时候就不用再去重写主题配置文件了。  

```shell
cd ~/git/CSTHexo
mkdir ./source/_data
cp ./themes/next/_config.yml ./source/_data/next.yml
```

然后编辑 `./source/_data/next.yml` 文件即可实现对主题的配置了。  

## 主题配置

修改主题的配置文件：  

```shell
vim ~/git/CSTHexo/source/_data/next.yml
```

为了使本文件生效，必须将 `override: false` 改为 `true`  

如果博客是建立在子目录中的，需要找到 `Menu Settings` 去除掉所有开头的 `/`  

配置头像（Avatar）需要在 `./source` 目录下建立 `uploads` 目录，为了方便，这个目录可以用来存放一些站点的基本图片：  

```shell
mkdir ./source/uploads
```

将图片放入这个目录后，在 `./source/_data/next.yml` 中找到 `# Sidebar Avatar` 填写图片的相对路径即可：  

```yaml
# Sidebar Avatar
avatar:
  # in theme directory(source/images): /images/avatar.gif
  # in site  directory(source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /uploads/avatar.jpg
  # If true, the avatar would be dispalyed in circle.
  rounded: true
  # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
  opacity: 1
  # If true, the avatar would be rotated with the cursor.
  rotated: false
```

# 分类、标签云、关于

[Next 主题的 Wiki](https://github.com/iissnan/hexo-theme-next/wiki) 提供了如何添加子页面。  
主题配置文件 `./source/_data/next.yml` 文件中 `menu:` 相关部分控制了各类文件的显示与否，`||` 后是图标名称，使用的是 [Font Awesome Icons](https://fontawesome.com/v4.7.0/icons/) 4.70 版本。  

## 分类页面

```shell
hexo new page categories
INFO  Created: ~/git/CSTHexo/source/categories/index.md # 提示建立成功
vim ~/git/CSTHexo/source/categories/index.md # 编辑
```

改为：  

```markdown
---
title: categories
date: 2018-09-01 13:32:35
type: "categories"
comments: false
---
```

这样就好了，同时关闭了分类页面的评论功能。  

然后去掉 `./source/_data/next.yml` 文件中 `menu:` 相关部分的注释即可。  

## 标签页面

标签页面也是类似：  

```shell
hexo new page "tags"
```

改为：  

```markdown
---
title: tags
date: 2018-09-01 13:42:34
type: "tags"
comments: false
---
```

然后去掉 `./source/_data/next.yml` 文件中 `menu:` 相关部分的注释即可。  

## 关于页面

```shell
hexo new page "about"
```

这次不需要再设置分类，只需要设置一下禁止评论即可：  

```markdown
---
title: about
date: 2018-09-01 13:47:43
comments: false
---
```

# DISQUS 评论系统

[Disqus](http://disqus.com) 注册完成后把 `Shortname` 填写到 `./_config.yml` 和 `./source/_data/next.yml` 中，方法如下：  

`./_config.yml` 中，在文末添加：  

```yaml
# Disqus
disqus_shortname: your-disqus-shortname
```

`./source/_data/next.yml` 中，找到 Disqus 配置的地方，修改为：  

```yaml
# Disqus
disqus:
  enable: true
  shortname: your-disqus-shortname
  count: true
  lazyload: false
```

Disqus 评论配置完成，如果需要在某篇博文中禁用评论，在md文件的front-matter中增加：  

```markdown
---
comments: false
---
```
