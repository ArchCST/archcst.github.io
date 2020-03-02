title: Java x D71 | Servlet
date: 2020-02-25
updated: 
comments: true
tags:
  - learn
  - Servlet
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
Mac 修改 Tomcat 使用的 JVM 版本
IDEA 运行 Tomcat 项目时的部署路径
{% endcq %}
<!--more-->

# Mac 修改 Tomcat 使用的 JVM 版本
Tomcat 使用的的 JVM 版本和 PATH 中指定的 `JAVA_HOME` 是一样的，如果需要指定 JVM 版本，可以在 `setclasspath.sh` 文件中设置。
先找到文件：
```shell
vim /Library/Tomcat/bin/setclasspath.sh
```

然后在顶部添加：
```shell
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home/
export JRE_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home/jre
```
添加之前应该看一下 JVM 目录是否正确。

# IDEA 运行 Tomcat 项目时的部署路径
网上很多目录都是有问题的，其实可以通过日志信息找到配置文件的路径
先运行项目，然后在 `Tomcat Catalina Log` 中找到这么一行信息：
```shell
26-Feb-2020 15:22:31.805 信息 [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /Users/cst/Library/Caches/IntelliJIdea2019.3/tomcat/Tomcat_8_5_51_LearnServlet
```
这个 `CATALINA_BASE` 就是 IDEA 存放当前项目配置信息的路径。可在 `CATALINA_BASE` 下的 `conf/Catalina/localhost` 中找到所有虚拟路径映射的 xml 文件，其中大写的 ROOT.xml 是根目录的映射。打开任意一个 xml 文件即可看到项目用于部署的物理存放路径 `path_to_project/artifacts/xxx_war_exploded`，这个物理地址被称为 `部署目录`

那么现在我们有几个目录：
```shell
CATALINA_HOME： /Library/apache-tomcat-8.5.51/ # 其实就是 tomcat 的安装目录
CATALINA_BASE： ~/Library/Caches//Users/cst/Library/Caches/IntelliJIdea2019.3/tomcat/Tomcat_8_5_51_LearnServlet/
项目目录：项目存放在磁盘上的物理路径
部署目录：项目目录/out/artifacts/xxx_war_exploded/
```

IDEA 做了以下几个事情：
1. 编译 `项目目录` 到 `部署目录` 中，其中 `项目目录/src` 将会编译到 `部署目录/WEB-INF/class`。
2. 生成了 CATALINA_BASE，并在其中存放了关于当前项目的全部 tomcat 配置信息，其中比较重要的是 `CATALINA_BASE/conf/Catalina/localhost` 中的 url 映射
3. 用 CATALINA_HOME 中的 catalina.sh 脚本运行 CATALINA_BASE