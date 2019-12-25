title: Mac OS 下使用 iTerm + Zsh + Oh-My-Zsh 打造完美终端  
date: 2018-09-16  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

<img src="/uploads/postpics/iTerm2.png" alt="img" width="" />

Iterm2 是 Mac OS X 下的一个终端模拟器，类似于 Terminal。但 Iterm2 比 Terminal 多了很多配置上的优势，界面更清晰美观，扩展性更好。  

<!-- more -->

# 下载安装 iTerm 2

[iTerm2 - macOS Terminal Replacement](https://www.iterm2.com/)  
下载完成后解压并移动到**应用程序**目录即可  

# 配置 iTerm 2

打开 iTerm 后按下`⌘+,*`，在Profiles中可以对按键、外观、字体等进行配置  

# 更新和配置 zsh

安装[homebrew](https://brew.sh/)   
homebrew国内访问比较慢，可以更换国内的镜像源  

## 更换 homebrew 中科大镜像源

```shell
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

因为要使用 `zsh` 作为默认 shell，所以先切换到 zsh 再更换 homebrew-bottles  

```shell
brew install zsh zsh-completions
zsh
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

## 设置 zsh 为默认 shell

```shell
sudo vim /etc/shells
```

在文件末尾添加一行：  

```shell
/usr/local/bin/zsh
```

再运行命令：  

```shell
chsh -s /usr/local/bin/zsh
```

此时可以重启 iTerm 看是否配置成功  

## 参考资料

[Homebrew国内加速 - 前端博客](https://www.noonme.com/post/2017/03/homebrew-speed-up/)  

# 安装使用 Oh-My-Zsh

先查看 zsh 的版本号：  

```shell
zsh --version
```

高于 `5.1.1` 即可，然后执行命令安装 oh-my-zsh：  

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

完成后即可使用 oh-my-zsh 了  

# 使用 Oh-My-Zsh 插件

Github 上的 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) ，上有常用的使用方法  
Oh-My-Zsh 提供了很多插件，但并未默认启用。所有的插件都在 `~/.oh-my-zsh/plugins` 目录内。例如启用 `git` 和 `osx` 插件：  

```shell
vim ~/.zshrc
```

在**66**行左右添加代码：  

```shell
plugins=(
git
osx
)
```

[Plugins · robbyrussell/oh-my-zsh Wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins) 这里有插件列表和说明。  

# 使用 Oh-My-Zsh 主题

打开配置文件  

```shell
vim ~/.zshrc
```

第十行左右的 `~ZSH_THEME="robbyrussell"~` 改一下主题名称即可。  
[Themes · robbyrussell/oh-my-zsh Wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/themes) 这里有主题列表和截图。  
大多数主体需要 [powerline](https://github.com/powerline/fonts) 的字体支持：  

```shell
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
cd ..
rm -rf fonts
```

# 参考资料

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)   
[Mac 下配置终端环境 iTerm2 + Zsh + Oh My Zsh + tmux | 明无梦](https://www.dreamxu.com/mac-terminal/)  
