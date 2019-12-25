title: 落格输入法使用 fcitx-remote-for-osx 实现 spacemacs 自动切换中英文  
date: 2018-09-05  
updated:  
comments: true  
tags:  

-   learn
-   emacs

layout: post  

---

Mac 下使用 Vim 或 Spacemacs 的用户可能需要在退出 Insert Mode 进入 Normal Mode 的时候自动切换回英文输入法，避免中文输入法截断快捷键的输入。 fcitx-remote-for-osx 插件可以实现这个功能。  

使用搜狗输入法只需要查阅 [fcitx-remote-for-osx 的官方 Github](https://github.com/CodeFalling/fcitx-remote-for-osx) 照搬即可，这里只提一下官方还没有支持的落格输入法如何实现。  

本文已被落格官方收录  

<!-- more -->

# 准备工作

MAS 安装 XCode，启用 Command Line Tool  

# 操作步骤

克隆 fcitx-remote-for-osx 仓库：  

```shell
$ cd ~/git
$ git clone https://github.com/CodeFalling/fcitx-remote-for-osx
$ cd fcitx-remote-for-osx
```

如果之前已经使用过 homebrew 安装过 fcitx-remote-for-osx 可以通过 `fcitx-remote -n` 查询到落格输入法的识别名称为 `com.logcg.inputmethod.LogInputMac.LogInputMacSP`，然后就可以自行编译了：  

```shell
$ xcodebuild GCC_PREPROCESSOR_DEFINITIONS='$GCC_PREPROCESSOR_DEFINITIONS CHINESE_KEYBOARD_LAYOUT=@\"com.logcg.inputmethod.LogInputMac.LogInputMacSP\"' install
```

编译中遇到以下报错：  

```shell
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

可以通过以下代码解决：  

```shell
$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

# 检测安装是否成功

使用以下代码检测是否安装成功：  

```shell
$ fcitx-remote -c # 切换回英文
Changing to com.apple.keylayout.US # 成功

$ fcitx-remote -o # 切换到中文
Changing to com.logcg.inputmethod.LogInputMac.LogInputMacSP # 成功
```

Enjoy  
