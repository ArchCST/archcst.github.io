title: Java x D
date: 2020-02-11
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
{% endcq %}
<!--more-->

# MariaDB 安装后的提示信息

```
MySQL is configured to only allow connections from localhost by default

To connect:
    mysql -uroot

To have launchd start mariadb now and restart at login:
  brew services start mariadb
Or, if you don't want/need a background service you can just run:
  mysql.server start
```

SQL 是有国际标准的，叫 `ANSI-SQL`

Q: 数据库存在那里，磁盘还是内存？
no-sql 产品：redis，也是一种数据库，这种数据库存的数据是存在内存中的，"no" 表示 "not-only"

大多数使用的 mysql 还是5.5, 5.7 版本，建议学习这些版本
最新版本 8+