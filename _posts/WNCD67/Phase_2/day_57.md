title: Java x D57 | MySQL
date: 2020-02-11
updated: 
comments: true
tags:
  - learn
  - MySQL
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">note</span>
创建学生表、教师表、课程表、成绩表

<span style="font-variant: small-caps;">homework</span>
创建淘宝数据库
{% endcq %}
<!--more-->

# 创建学生表、教师表、课程表、成绩表
```sql
# 连接数据库
CREATE DATABASE student;
USE student;

# 创建四张表
create table student(
sno char(10) primary key not null,
sname varchar(20) not null,
sex char(1) not null,
birthday date,
home varchar(20),
blood enum('A','B','AB','O')
);

create table teacher(
tno char(10) primary key,
tname varchar(20) not null,
sex char(1) not null,
birthday date,
prof varchar(10),
department varchar(20)
);

create table course(
cno char(10) primary key not null,
cname varchar(10) not null,
tno char(10) not null
);

create table score(
sno char(10) not null,
cno char(10) not null,
score decimal(4,1) not null,
foreign key (sno) references cst_student(sno),
foreign key (cno) references cst_course(cno)
);
```

# 创建淘宝数据库
```sql
# 连接数据库
CREATE DATABASE taobao;
USE taobao;

# 用户表
CREATE TABLE user(
uid BIGINT PRIMARY KEY AUTO_INCREMENT, -- 用户ID
name VARCHAR(16) NOT NULL UNIQUE, -- 用户名
password CHAR(128) NOT NULL,  -- 哈希后的用户密码
sex ENUM('M','F'), -- 性别，可以不填
birthday DATE, -- 生日
cellphone CHAR(11) UNIQUE, -- 手机号，同一手机号只能注册一个账号
last_login TIMESTAMP, -- 最后登录系统的时间
sid INT UNIQUE -- 店铺ID，可以没有，但同一个店铺只能有一个掌柜
);

# 商品表
CREATE TABLE merchandise(
mid BIGINT PRIMARY KEY AUTO_INCREMENT, -- 商品ID
sid INT NOT NULL, -- 所属店铺
title VARCHAR(64) NOT NULL, -- 商品标题
price DECIMAL(12,2) DEFAULT '0.50' NOT NULL, -- 商品价格最高99亿元，默认0.5元
cid SMALLINT, -- 分类ID
detail TEXT -- 商品详细描述
);

# 商铺表
CREATE TABLE shop(
sid INT PRIMARY KEY AUTO_INCREMENT, -- 店铺ID
name VARCHAR(64) NOT NULL, -- 店铺名
credit TINYINT NOT NULL, -- 信誉度
uid BIGINT NOT NULL UNIQUE -- 掌柜ID，每个账号只能有一个店铺
);

# 分类表
CREATE TABLE category(
cid SMALLINT PRIMARY KEY, -- 分类ID，不自增
cname VARCHAR(64) NOT NULL, -- 分类名称
parent_id SMALLINT NOT NULL -- 父级分类ID，顶级分类的父级ID为0
);

# 订单
CREATE TABLE indent(
oid BIGINT PRIMARY KEY AUTO_INCREMENT, -- 订单号
uid BIGINT NOT NULL, -- 消费者ID
mid BIGINT NOT NULL, -- 消费的商品ID
price DECIMAL(12,2) NOT NULL, -- 成交金额
t_order DATETIME NOT NULL, -- 下单时间
t_purchase DATETIME, -- 支付时间
t_close DATETIME, -- 交易完成时间
remark VARCHAR(255), -- 备注
eid BIGINT UNIQUE -- 对应的评价，可以没有评价，但评价ID唯一
);

# 评价
CREATE TABLE evaluate(
eid BIGINT PRIMARY KEY AUTO_INCREMENT, -- 评价编号
oid BIGINT NOT NULL UNIQUE, -- 对应的订单，不能为空且一个订单只能有一个评价
rate TINYINT DEFAULT 5 NOT NULL, -- 评价星级，默认五星好评
comment TEXT -- 评论文字
);

# 添加外键
ALTER TABLE user ADD FOREIGN KEY (sid) REFERENCES shop(sid); -- 用户表的店铺ID外键
ALTER TABLE merchandise ADD FOREIGN KEY (cid) REFERENCES category(cid); -- 商品表的分类ID外键
ALTER TABLE shop ADD FOREIGN KEY (uid) REFERENCES user(uid); -- 店铺表的掌柜ID外键
ALTER TABLE indent ADD FOREIGN KEY (uid) REFERENCES user(uid); -- 订单表的消费者ID外键
ALTER TABLE indent ADD FOREIGN KEY (mid) REFERENCES merchandise(mid); -- 订单表的商品ID外键
ALTER TABLE indent ADD FOREIGN KEY (eid) REFERENCES evaluate(eid); -- 订单表的评价ID外键
ALTER TABLE evaluate ADD FOREIGN KEY (oid) REFERENCES indent(oid); -- 评价表的订单ID外键
```