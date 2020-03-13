title: 腾讯云服务器搭建 Java 生产环境
date: 2020-03-07
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
本来是在阿里云购买的，新用户活动折扣也还不错好，但注册下来因为系统Bug导致RDS和ECS不在一个区域，数据库必须配置公网访问，给速度和安全性都带来了隐患。联系客服通知我无法迁移数据库区域，也无法退款，简直伤心。
商榷之后阿里云给的处理意见是服务器搬不了区域，最多只能给你退款，而且已经使用的天数是要扣费的，并要求你确认你将无法再参加新用户优惠。呵呵，拜拜。

## 待办列表：
- [x] 域名购买
- [x] 腾讯云服务器购买
- [x] 添加常用电脑的 ssh 公钥到服务器
- [x] JRE 安装配置
- [x] MySQL 安装配置
- [x] Tomcat 安装配置
- [x] Git 服务器搭建
- [x] nginx 反向代理
- [ ] 二级域名配置博客
- [ ] 博客迁移
- [ ] 几个服务的开机自启
- [ ] 博客的迁移
- [] 域名备案

## 环境信息
```shell
cat /etc/redhat-release
CentOS Linux release 7.7.1908 (Core)
```

# 添加常用电脑的 ssh 公钥到服务器
## 服务器修改密码和加密钥对
左侧侧边栏 SSH密钥 -> 创建密钥 -> 创建新密钥对，随便写一个名称点击确定，然后浏览器会自动下载私钥。
[CVM控制台](https://console.cloud.tencent.com/cvm/) 中实例地区选择购买的实例的地域，在实例列表中找到实例然后点右侧的 `更多` -> `密码/密钥` -> `重置密码` 可以把 root 账户的密码改了，然后点 `加载密钥` 选择刚才创建的密钥对点击确定，这样就把公钥绑定到了实例上。

## 配置安全组
我的实例莫名其妙的是全部放行的，但是我目前只需要开放这几个端口：
| 端口号 | 用途           |
| ------ | -------------- |
| 22     | SSH连接        |
| 80     | http 默认端口  |
| 443    | https 默认端口 |
| 3306   | MySQL 远程访问 |

在 CVM 控制台左侧侧边栏选择 `安全组`，将原有规则删掉，然后点击添加规则，在左侧 `类型`  中依次选择添加即可，CVM 这边做得很傻瓜化了。

## 在本机配置 ssh
将下载好的私钥文件放电脑的到 `~/.ssh` 目录中，终端中修改私钥文件的属性为 **可读不可写不可执行**：
```shell
chmod 400 ~/.ssh/文件名
```

修改 ssh 的 config 文件：
```shell
vim ~/.ssh/config
```

添加内容：
```shell
# 注释以井号开始
Host // 随意，之后将通过这个名字连接 CVM
HostName // CVM 实例的公网IP地址
Port 22 // 端口号，默认为22
User root   // ssh 登录账号
IdentityFile ~/.ssh/文件名 // 私钥文件在本机的地址
```

之后就可以在终端中使用 `ssh [Host]` 直接连接到服务器了。第一次连接问问你是否添加服务器到本机 ssh 的 known_hosts 列表，输入 yes 即可。

# JRE 安装
相对于在 yum 上安装 openjdk，我更倾向于在服务器上安装 Oracle 的 JRE8，由于 Oracle 用 wget 直接下载文件无法输入账号密码，所以只能先下载到本地电脑上再通过 ssh 的 scp 工具传到服务器上。

先在服务器上创建一个目录用来存放软件安装包：
```shell
# 新服务器首先手动update一下
yum update
# 创建包上传目录
mkdir /root/packages
```

到 [Oracle 官网](https://www.oracle.com/java/technologies/javase-server-jre8-downloads.html) 下载 JRE8，难得注册账号可以百度一下。

我这里出现一个奇怪的现象，用 Chrome 登陆 Oracle 时 Oracle 的 Single sign on 服务报错，换成 FireFox 居然就好了- -

下好了之后在本地通过 scp 把 .gz 压缩包直接发到服务器上：
```shell
# [Host] 直接用 ~/.ssh/config 中配置的的 Host 名就行
scp ~/Downloads/server-jre-8u241-linux-x64.tar.gz [Host]:/root/packages
```
CVM 的速度还不错，成都发到上海 scp 还是能跑到 8M 左右的速度，之前搞 Linode 的海外服务器差不多能跑到几十 k 就不错了。

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

在终端输入 `startup.sh` 启动 tomcat，此时在浏览器中输入你的 `公网IP` 应该就能看到汤姆猫了。

# MySQL 安装配置
这次买的基本款没有附赠数据库实例，所以需要在 CVM 上自己安装 MySQL 使用。
首先将 CentOS7 自带的 mariadb 移除：
```shell
yum remove -y mariadb-libs
```

然后添加 MySQL 5.7 的 yum 仓库并安装 MySQL 5.7：
```shell
# 下载仓库
wget https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
# 安装仓库
yum localinstall mysql57-community-release-el7-11.noarch.rpm
# 安装 MySQL
yum install mysql-community-server
```
下载大约200M的文件，这个过程就比较缓慢了。

安装完成后通过系统服务启动 MySQL 服务器：
```shell
systemctl start mysqld
```
然后通过日志文件查看 MySQL 的初始密码：
```shell
grep "password" /var/log/mysqld.log
```

找到这条记录把密码复制下来：
```shell
[Note] A temporary password is generated for root@localhost: xxxxxxxx
```

然后登录到MySQL修改密码：
```shell
mysql -p
# 输入刚才复制的密码回车后进入MySQL命令提示符
mysql> set password=password('新密码'); 
```
如果提示『Your password does not satisfy the current policy requirements』说明密码设置得太简单了，可以通过命令查看默认的密码策略如下：
```shell
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password_check_user_name    | OFF    |
| validate_password_dictionary_file    |        |
| validate_password_length             | 8      |
| validate_password_mixed_case_count   | 1      |
| validate_password_number_count       | 1      |
| validate_password_policy             | MEDIUM |
| validate_password_special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.00 sec)
```
也就是说，需要八位以上同时包含大小写字母、数字、符号。

# Gitea 服务器搭建
## 安装 Git
先安装 git
```shell
yum -y install git
```
创建 git 组，然后创建一个 git 用户，这一个系统用户，不需要登录功能
```shell
groupadd git
useradd git -g git -s /bin/false
```
## 二进制安装 gitea
把二进制文件下载到 bin 目录里：
```shell
cd /usr/local/bin
wget -O gitea https://dl.gitea.io/gitea/1.11.2/gitea-1.11.2-linux-amd64
# 修改运行权限
chmod +x gitea
chown -R git:git gitea
```
## 创建 gitea 的工作目录
我打算把目录放在 `/usr/local/gitea/` 下：
```shell
mkdir -p /usr/local/gitea/{custom,data,log}
chown -R git:git /usr/local/gitea/
chmod -R 750 /usr/local/gitea/
```
## 创建 gitea 配置文件目录
Linux 中 `etc` 下都是存放的各种服务的配置文件，所以 gitea 的配置文件也放在这里比较好：
```shell
mkdir /etc/gitea
chown root:git /etc/gitea
chmod 770 /etc/gitea
```
## 服务方式启动 Gitea
首先创建一个 Gitea 的服务描述文件
```shell
touch /etc/systemd/system/gitea.service
vim /etc/systemd/system/gitea.service
```
然后把[官方样例](https://github.com/go-gitea/gitea/blob/master/contrib/systemd/gitea.service)的所有内容全部粘贴进去。
把工作目录改成我的工作目录，有两个地方：
```shell
WorkingDirectory=/usr/local/gitea/
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/usr/local/gitea
```
取消掉 MySQL 前面的注释，
```shell
Requires=mysql.service
```
然后就可以启动 Gitea 服务了：
```shell
# 添加开机启动
systemctl enable gitea
# 启动服务
systemctl start gitea
```
## 解决报错 Failed to start gitea.service: Unit not found.
此时我遇到了报错，错误信息为 `Failed to start gitea.service: Unit not found.`，是说某个服务模块没有找到。
因为我只取消注释了 MySQL 这一个服务，那么问题应该就是在 MySQL 服务上。
```shell
# 试试查询MySQL服务文件的路径
locate mysql.service
# 发现没有输出，再试试找 deamon 文件
locate mysqld.service
# 找到了两个输出
```
于是把 修改 `/etc/systemd/system/gitea.service` 文件，把 `Requires=mysql.service` 改为 `Requires=mysqld.service`，

然后再次启动服务，没有报错就是最好的消息。

## 配置安全组，放行 Gitea 默认的 3000 端口
回到 [CVM控制台](https://console.cloud.tencent.com/cvm/)，点击左侧边栏的安全组，点击默认安全组ID进入配置，按下图添加规则：
![放行 Gitea 默认端口](https://i.loli.net/2020/03/10/5RFdVp7CuDHO4UB.png)

保存后通过本地浏览器访问你的 `公网IP:3000` 就可以看到 Gitea 的主页了。

# 反向代理

我打算把所有的子项目，比如 Blog、Gitea 和 将来会部署的一些小玩意儿都统统分配到二级域名下，然后把 @ 和 www 解析到 blog 二级域名。
个人小服务器当然不可能买那么多服务器，那么实现方式就只有一种：为每一个项目分配不同的端口号，然后通过一个反向代理根据请求 url 转发到对应的端口上。
于是还差一个 Nginx 来做反向代理。
## 把 Tomcat 改回 8080 端口
```shell
vim $CATALINA_HOME/conf/server.xml
```

输入 `/80` 搜索一下找到：
```shell
<Connector port="80" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```
把 port 号改为 `8080` 。然后保存退出，然后 `shutdown.sh` 加上 `startup.sh` 重启 tomcat 即可。

## 安装并启动 Nginx
```shell
# 安装
yum install nginx
# 设置开机启动
systemctl enable nginx
# 启动服务
systemctl start nginx
```
这时访问 `公网IP`，访问到 CentOS 欢迎页面就对了。

## 域名解析
我打算这么搞：
git.archcst.cn | 访问我的 Gitea
tom.archcst.cn | 访问我的 Tomcat
archcst.cn | 重定向到 blog.archcst.cn，以后可能做个首页
blog.archcst.cn | 访问我的博客

由于物理上都位同一个服务器，那么只需要解析到同一个公网IP上即可，之后通过 Nginx 来控制反向代理。
于是在域名服务商那里解析成：

## 配置反向代理
CentOS 下和 Ubuntu 有一定的区别，虚拟服务器的配置文件应该放到 /etc/nginx/conf.d 中，一般用 `二级域名.conf` 这种形式，后缀名必须是 `conf`。
创建 Tomcat 和 Gitea 的反向代理文件：
```shell
touch /etc/nginx/conf.d/tomcat.conf
touch /etc/nginx/conf.d/gitea.conf
```
编辑这两个文件，填上如下内容：
```shell
# /etc/nginx/conf.d/gitea.conf
server {
  listen 80;
  server_name git.archcst.cn; # 填写全域名路径
  location / {
    proxy_set_header    X-Real-IP $remote_addr;
    proxy_set_header    Host      $http_host;
    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass          http://localhost:3000; # 填写端口号
  }
}

# /etc/nginx/conf.d/tomcat.conf
server {
  listen 80;
  server_name tom.archcst.cn; # 填写全域名路径
  location / {
    proxy_set_header    X-Real-IP $remote_addr;
    proxy_set_header    Host      $http_host;
    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass          http://localhost:8080; # 填写端口号
  }
}

# /etc/nginx/conf.d/tomcat.conf
server {
  listen 80;
  server_name tom.archcst.cn; # 填写全域名路径
  location / {
    proxy_set_header    X-Real-IP $remote_addr;
    proxy_set_header    Host      $http_host;
    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass          http://localhost:8080; # 填写端口号
  }
}
```

两个文件保存退出后 `systemctl restart nginx` 重启服务器，没报错就成功了，然后可以用本地浏览器打开配置的两个二级域名看看效果。
## 双域名解析到博客
目前 Blog 还是用 [Hexo](hexo.io) 搭建的，所以一个静态服务器即可满足需求，直接用 Nginx 指向博客的仓库就可以了。

## 域名备案
阿里云上需要在域名实名认证3天后才可以开始备案。
打开 [阿里云备案服务](https://beian.aliyun.com/) ，登录后点击开始备案。