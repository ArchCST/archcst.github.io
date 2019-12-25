title: Cheatsheet Flask 虚拟环境搭建  
date: 2019-06-27  
updated: 2019-06-27  
comments: true  
tags:  

-   cheatsheet

layout: post  

---

本文内容：  
在项目文件夹内创建虚拟环境目录。  
安装 `pipenv` 来管理虚拟环境内的包、包依赖。  
通过 `pipenv` 安装 `flask` `python-dotenv` `watchdog`  
启动 `flask` 的调试模式  
设置 `FLASK_APP` 环境变量  
在 `emacs` 中启用虚拟环境  

<!--more-->


# 创建虚拟环境

```shell
$ python3 -m venv ENV_NAME
```

其中 `ENV_NAME` 是虚拟环境目录名，一般用 `.venv` ，所以应该为：  

```shell
$ python3 -m venv .venv
```


# 启用虚拟环境

进入到项目根目录后：  

```shell
$ source ENV_NAME/bin/activate
```

上面使用了 `.venv` 作为目录名，那么这里应该为：  

```shell
$source .venv/bin/activate
```

这时候可以看到提示符 `$` 旁有 `(venv)` 的标示，表示已经启用了虚拟环境了。  

退出虚拟环境直接输入：  

```shell
$ deactivate
```


# 安装 pipenv

```shell
$ pip install pipenv
```

Linux 和 macOS 下需要 `sudo`  

```shell
$ pipenv --version
```

可以看到 `pipenv` 的版本号，表示安装成功  


# 安装 flask、python-dotenv、watchdog

```shell
$ pipenv install flask
$ pipenv install python-dotenv
$ pipenv install watchdog --dev
```

其中 `watchdog` 是重载器，就是在编辑完代码之后不需要重新启动服务就可以自动重载的工具，是只有在 `开发模式` 下才会启用的包，称为 `开发依赖` ， `--dev` 就是申明这个包为 `开发依赖` 的。  

```shell
$ pipenv install flask-wtf
$ pipenv install flask-sqlalchemy
$ pipenv install flask-migrate
$ pipenv install flask-mail
```

还有一些常用的包：  

| 包名称             | 说明                       |
| bootstrap-flask    | 内置bootstrap资源和快速渲染HTML组件的宏 |
| flask-moment       | 本地化日期和时间吗，集成了 moment.js |
| faker              | 生成虚拟数据，开发时用来填充数据库的 |
| flask-debugtoolbar | 调试器                     |
| flask-login        |                            |


# 配置 flask 开发环境

在项目的根目录创建两个文件 `.env` `.flaskenv` ，其中 `.env` \*用来存储私有的，敏感的环境变量，这个文件一定不能上传到公共仓库或者其他公开的地方\*。 `.flaskenv` 用来存储和 flask 相关的公开环境变量。这两个文件用 `#` 表示注释。  

在 `.flaskenv` 文件中添加以下内容，开启开发环境。  

```
FLASK_ENV=development
```

开启后执行 `flask run` 会自动激活调试器和重载器。  

另外，默认 `flask run` 会自动查找 `app.py` 文件，但如果没有使用 `app.py` 作为程序主模块的名称，那么还需要在 `.flaskenv` 中添加以下内容来制定主模块的名称：  

```
FLASK_APP=主模块名称
```


# 看看效果

到这里 `flask` 的基本配置就算完了，可以在程序根目录中 `app.py` 代码用于测试：  

然后在终端中启动服务：  

```shell
# 启用虚拟环境
$ source venv/bin/activate

# 启动 Flask 服务
$ flask run
```

服务启动后会提示：  

```shell
 * Serving Flask app "app.py" (lazy loading)
 * Environment: development
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with fsevents reloader
 * Debugger is active!
 * Debugger PIN: 137-376-833
```

打开浏览器输入地址 `http://127.0.0.1:5000/` 即可看到 `hello flask!` ，配置完成。  

这时把 `app.py` 中的返回值改为 `"HELLO FLASK!"` 保存后不用重新启动服务，直接刷新浏览器即可看到改变，说明 `watchdog` 重载器起作用了。  

根据提示，在终端中输入 `Ctrl+c` 即可停止服务。  


# 常用的一些内置命令

```shell
# 查看所有已注册的路由
flask routes

# 查看已安装的包的版本
pip list

# flask shell
flask shell
```


# pipenv 的一些细节


## 将虚拟环境安装到项目内

pipenv 创建的虚拟文件夹默认会装到 `~/.local/share/virtualenvs/` 目录中，如果想在项目目录内创建，需要设置环境变量：  

```shell
export PIPENV_VENV_IN_PROJECT=1
```

然后可以使用 `env` 命令查看环境变量，应该有 `PIPENV_VENV_IN_PROJECT=1` 这一行。  
这时再执行 `pipenv install` 命令来安装 pipfile 就可以将虚拟环境装到 `/path/to/project/.venv/` 中了。  
如果需要永久设置则需要在 `.zshrc` 中添加 `export PIPENV_VENV_IN_PROJECT=1` 即可。（zsh）  


## 指定 pipenv 安装时使用的 python 版本

```shell
# 使用 pipenv 安装 pipfile 时指定 python 版本
pipenv install --python 2.7

# 安装开发依赖
pipenv install --dev
```


# emacs 中激活虚拟环境

用了一段时间 pycharm 之后还是觉得 emacs 更顺手一些，所以还是配置了 spacemacs 作为 flask 的编辑器。不用 emacs 的话完全不要看这段，也看不懂。  

```emacs-lisp
;; python configurations:
(require 'pyvenv)
(pyvenv-activate "~/.pyvenv/Py_3.7")
(global-company-mode)
(add-to-list 'company-backends 'company-anaconda)
(add-to-list 'company-backends 'company-jedi) ;; 配合 jedi 包使用，安装方法：pipenv install jedi
(add-to-list 'company-backends 'company-web)
(add-hook 'python-mode-hook 'anaconda-mode)

;; 因为我的所有项目的虚拟环境均为项目根目录下的 venv 目录，所以写了这一句来根据项目根目录路径来实现打开任意项目文件的同时激活该项目的虚拟环境。
;; 需要目录内有 .projectile 支持，具体 Google 一下 projectile.el 这个 emacs 包。
;; TODO 问题是切换到另一个项目之后会更换虚拟环境，再切换回来也不会回到之前的虚拟环境。hook 应该做到 buffer 切换上，不过有些影响性能。我同时编辑两个项目的可能性目前还没有，所以暂时就这样了。
(add-hook 'python-mode-hook (lambda () (pyvenv-activate (concat (projectile-project-root) "/.venv")))) 

(setq tab-width 4)
(setq py-indent-offset 4)
(setq python-spacemacs-indent-guess nil)
(setq python-indent-guess-indent-offset nil)
(setq org-confirm-babel-evaluate nil)
```


# 遗留问题

emacs 中对 SQLAlchemy 的支持不好，部分代码没有出现补全。和李辉老师沟通后发现应该和SQLAlchemy的构造方法有关，Jedi 支持不了，暂时也就解决不了。
