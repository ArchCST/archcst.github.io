Mac 下安装 Visual Studio Code 并进行一些基础的配置
# 安装 vim
在 `Extentions` 中搜索 `vim` 就可以找到 vs code 的 vim 插件，安装即可
## 配置输入法自动切换
这里配置从 insert mode 回到 normal mode 时自动切换为英文。避免输入法拦截 vim 按键。

vim 插件支持 `im-select` 输入法切换器，首先要安装 `im-select` 这个插件。打开终端输入：
```shell
curl -Ls https://raw.githubusercontent.com/daipeihust/im-select/master/install_mac.sh | sh
```
这条命令将会把 im-select 安装到 `/usr/local/bin/` 中，可以用 `ls -al /usr/local/bin/im-select` 验证一下是否安装成功。
很快安装完成后，切换到英文输入法，在终端中输入 `im-select` 并回车就可以看到英文输入法的名称，复制下来。
点击 vim 的齿轮图标 -> Extension Settings 进入 vim 的配置页面，在 `Auto Switch Input Method: Default IM` 中粘贴刚才获取到的英文输入法的名称。
然后勾选 `Switch input method automatically when mode changed`。
剩下两个 `Obtain IMCmd` 和 `Switch IMCmd` 分别设置为 `/usr/local/bin/im-select` 和 `/usr/local/bin/im-select {im}` 即可。
# 一些好用的插件
Clock in status bar: 状态栏显示时间，[这里](https://github.com/felixge/node-dateformat)有格式化字符串的说明