title: 阿里云服务器搭建 Java 生产环境
date: 2020-03-03
updated: 
comments: true
tags:
  - learn
  - operation
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
{% endcq %}
<!--more-->

# 待办事件
- [x] 域名购买
- [x] ECS 服务器购买
- [x] 添加常用电脑的 ssh 公钥到服务器
- [x] JRE 安装配置
- [x] MySQL 安装配置
- [x] Tomcat 安装配置
- [] Git 服务器搭建
- [] 域名备案

# 添加常用电脑的 ssh 公钥到服务器
## 服务器修改密码和添加密钥对
先把服务器的密码改成自己的，在管理控制台左侧菜单 -> 实例与镜像 -> 实例中点击实例ID打开实例，基本信息框右侧 `更多` 里面找到重置实例密码。
在  [管理控制台](https://ecs.console.aliyun.com/) 左侧菜单栏 -> 网络与安全 -> 密钥对点击右上角创建密钥对。创建完成后会自动下载 .pem 文件。
在密钥对管理界将密钥对绑定到购买的 ECS 服务器上。绑定之后就可以重启一次 ECS 使密码修改和密钥对绑定生效。

## 配置防火墙安全组
为了确保 ssh 能用，先需要在 `左侧菜单 -> 网络与安全 -> 安全组` 点击默认生成的安全组，确认 22 端口策略为允许，之后要部署 Tomcat 肯定是需要 TCP 也是允许的，IMCP 控制服务器是否能被 PING 到。
在 `安全组` 后面的 `管理实例` 可以确定购买的 ECS 在这个安全组中。

## 在本机配置 ssh
将下载好的 `.pem` 私钥文件放电脑的到 `~/.ssh` 目录中，终端中修改私钥文件的属性为 **可读不可写不可执行**：
```shell
chmod 400 ~/.ssh/xxx.pem
```

修改 ssh 的 config 文件：
```shell
vim ~/.ssh/config
```

添加内容：
```shell
# 注释以井号开始
Host // ECS实例的名称
HostName // ECS实例的公网IP地址
Port 22 // 端口号，默认为22
User root   // ssh 登录账号
IdentityFile ~/.ssh/xxx.pem // .pem私钥文件在本机的地址
```

之后就可以在终端中使用 `ssh [Host]` 直接连接到服务器了。第一次连接问问你是否添加服务器到本机 ssh 的known_hosts列表，输入 yes 即可。

# JRE 安装
相对于在 yum 上安装 openjdk，我更倾向于在服务器上安装 Oracle 的 JRE8，由于 Oracle 用 wget 直接下载文件无法输入账号密码，所以只能先下载到本地电脑上再通过 ssh 的 scp 工具传到服务器上。

先在服务器上创建一个目录用来存放软件安装包：
```shell
# 新服务器还是手动update一下
yum update
# 创建包上传目录
mkdir /root/packages
```

到 [Oracle 官网](https://www.oracle.com/java/technologies/javase-server-jre8-downloads.html) 下载 JRE8，难得注册账号可以百度一下。

下好了之后在本地通过 scp 把 .gz 压缩包直接发到服务器上：
```shell
# [Host] 直接用 ~/.ssh/config 中配置的的 Host 名就行
scp ~/Downloads/server-jre-8u241-linux-x64.tar.gz [Host]:/root/packages
```

登入远程服务器，然后解压：
```shell
# 创建 java home 目录
mkdir /usr/local/java
# 解压到这个目录
tar -zxvf server-jre-8u241-linux-x64.tar.gz -C /usr/local/java/
# 查看解压成功与否，能看到 jdk1.8.0_241 目录
ls -al /usr/local/java
```

配置环境变量：
```shell
vim /etc/profile
```

在文件尾部添加：
```shell
# Java
export JAVA_HOME=/usr/local/java/jdk1.8.0_241
export PATH=$JAVA_HOME/bin:$PATH
```

source 一下使配置生效：
```shell
source /etc/profile
```

现在应该已经配置好了 Java 的运行环境，可以用 `java -version` 查看一下输出是否正确。


# MySQL 安装配置
我买的套餐是有送一个数据库RDS的，所以可以直接通过 [RDS控制台](https://rdsnext.console.aliyun.com/) 使用这个数据库。
## 设置数据库访问白名单
首先需要把 ECS 的内网IP添加到数据库连接白名单中，否则服务器无法访问到数据库。
我们先找到 ECS 的内网IP，在 ECS 控制台中点击实例ID，配置信息中游一个 `私有IP` 复制下来。
打开进入 RDS 控制台 -> 数据库实例ID -> 左侧的数据安全性 -> default 右侧的修改，删掉127.0.0.1，把ECS的私有IP粘贴进去保存。
需要本地电脑访问的话给自己本地电脑添加一个白名单分组，把本地电脑的公网IP（百度 `我的ip`）粘贴进去保存，然后点击左侧基本信息，`外网地址` 那里点一下申请，等会就可以通过本地电脑访问数据库了。
左侧点击数据库连接，右边修改链接地址可以自己指定连接地址。
## 设置数据库高权限账号
一个数据库只能配置一个[高权限账号](https://help.aliyun.com/document_detail/87038.html?spm=5176.13597150.202.2.223a1450mCWcOq)，这个高权限账号拥有数据库的最高权限。
点击 RDS 控制台左侧的账号管理创建账号即可，顺便可以创建几个普通账号给生产环境使用。

账号建立之后，可以在本地电脑上测试一下是否可以正常登陆：
```shell
mysql -h xxx.mysql.rds.aliyuncs.com -u [用户名] -p [密码]
```

# Tomcat 安装配置
依然采用下载解压包的方式，[Apache官网](https://tomcat.apache.org/download-80.cgi) 下载好压缩包后通过 scp 上传到ecs服务器：
```shell
scp ~/Downloads/apache-tomcat-8.5.51.tar.gz  [HOST]:/root/packages
```

然后解压到安装目录：
```shell
# 解压
tar -zxvf apache-tomcat-8.5.51.tar.gz -C .
# 移动到 /usr/local 下
mv apache-tomcat-8.5.51 /usr/local/tomcat
# 检查一下路径是否正确
ls /usr/local/tomcat/
```

`vim /etc/profile` 在尾部添加环境变量：
```shell
# Tomcat
export CATALINA_HOME=/usr/local/tomcat
export PATH=$CATALINA_HOME/bin:$PATH
```

退出 vim 后 `source /etc/profile` 使之生效。然后修改 tomcat 的端口号为 80：
```shell
vim $CATALINA_HOME/conf/server.xml
```

输入 `/8080` 搜索一下找到：
```shell
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```
把其中的 `8080` 改为 `80` 保存退出。

在终端输入 `startup.sh` 启动 tomcat，因为防火墙问题，目前还访问不到。

## ECS 配置安全组开放 80 端口
[ECS 控制台](https://ecs.console.aliyun.com/) 中点击左侧菜单栏 -> 网络与安全 -> 安全组，点击安全组ID进入配置页面，点右侧 `添加安全组规则` 然后按如下配置：
![ECS 开放 80 端口](https://i.loli.net/2020/03/05/I4lA8FMPNWtbfod.png)

确定后找到你的 `公网IP` ，复制到浏览器中打开就能看到汤姆猫了。

# Git 服务器搭建

# 域名解析
在 [域名控制台](https://dns.console.aliyun.com/) 中点击域名添加A记录，主机记录用 `@` 的话表示前面不加 `www` ，`记录值` 填写 ECS 的公网IP即可。
确定后稍等两分钟就可以通过域名访问到 tomcat 了。
## 域名备案
阿里云上需要在域名实名认证3天后才可以开始备案。
打开 [阿里云备案服务](https://beian.aliyun.com/) ，登录后点击开始备案。