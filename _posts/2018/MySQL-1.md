title: MySQL 入门指南  
date: 2018-09-16  
updated: 2020-02-11
comments: true  
tags:  
 - learn
 - MySQL
layout: post  
---

MySQL 语句是不区分大小写的，但是为了区分保留字和变量名，一般把保留字大写，变量和数据小写。  
本文是以前学习 MySQL 时写的，非常基础，内容也比一般的教程更详细一些，基本上操作完本文内容，就对数据库有了一个直观的了解了。  

<!-- more -->

# MySQL 的注释方法：

学习一门语言必须先学会注释，如果暂时看不懂下面内容，暂时掌握 `#` 注释即可。  

```sql
mysql> SELECT 1+1;     # 到本行行尾的注释
mysql> SELECT 1+1;     -- 到本行行尾的注释，"-- "注意空格
mysql> SELECT 1 /* 行间注释 */ + 1;
mysql> SELECT 1+
/*
多行
注释
*/
1;
```


# 安装和启动 MySQL 服务

## Ubuntu

```shell
# 安装 MySQL 服务器端和客户端
sudo apt-get install mysql-server
sudo apt-get install mysql-client

# 启动 MySQL 服务
sudo service mysql start
```

## CentOS 7

```shell
# 安装 MySQL 的分支 MariaDB
sudo yum install mariadb-server mariadb

# 启动 MariaDB
sudo systemctl start mariadb    #启动MariaDB
sudo systemctl stop mariadb     #停止MariaDB
sudo systemctl restart mariadb  #重启MariaDB
sudo systemctl enable mariadb   #设置开机启动
```

MariaDB 数据库管理系统是 MySQL 的一个分支，主要由开源社区在维护，采用 GPL 授权许可。  
开发这个分支的原因之一是：甲骨文公司收购了 MySQL 后，有将 MySQL 闭源的潜在风险，因此社区采用分支的方式来避开这个风险。  
MariaDB 的目的是完全兼容 MySQL，包括 API 和命令行，使之能轻松成为 MySQL 的代替品。  

# 数据库的基本概念
数据库：简单来说一般一个软件（特别是和数据有关的软件）会把它所拥有的 `所有数据`
放在一个 `数据库` 中，举个栗子来说，学生信息管理软件就拥有一个数据库，其中包含了
所有学生的信息。
表：每个数据库都有多张表，每张表都是个二维结构，大概可以把它看成是一张 Excel 的
表格。这个 Excel 的每一列对应着数据库的一个 `字段`，比如学生基本信息表就有字段 `学号、姓名、班级` 等等。而这个 Excel 表中的每一行都是一条 `数据`，比如
`0001、杰伦·小眼·周 、三年二班` 。可以看到，`数据` 和 `字段` 是严格一
一对应起来的。

SQL语句分为四种：
DDL: Data Definition Language 数据库定义语句
DQL: Data Query Language 数据库查询语句
DCL: 数据控制语句
DML: 数据操纵语句


# MySQL 数据库的基本使用方法

使用 root 登陆数据库管理软件（DBMS: database managment service），注意此 root 并非 Linux 的 root，而是 MySQL 的 root：

```shell
# 登录本机数据库
mysql -u root -p # 回车后提示输入数据库密码

# 如果是远程数据库需要加 -h
mysql -u root -h 服务器ip -p

# 如果还要指定数据库端口
mysql -u root -h 服务器ip -P 服务端口 -p
```


查看本机数据库：  

```sql
mysql> SHOW databases;
```

连接至其中一个数据库：  

```sql
mysql> USE 数据库名;
```

出现`database changeed`即表示连接成功  

查看数据库中的表：  

```sql
mysql> SHOW tables;
```

退出 MySQL ：  

```sql
mysql> EXIT
```

删除数据库：  

```sql
mysql> DROP DATABASE 数据库名;
```

删除数据库中的表：  

```sql
mysql> DROP TABLE 表名;
```

# 新建数据库和数据表

以下代码块中省略 `mysql>` 提示符。  

创建数据库和连接数据库的语法：  

```sql
CREATE DATABASE 数据库名;
USE 数据库名;
```

实际操作创建并连接一个数据库 `db_test`：  

```sql
CREATE DATABASE db_test;
USE db_test;
```

在数据库中新建一个表的语法：  

```sql
CREATE TABLE 表名
(
字段名 数据类型(数据长度),
字段名 数据类型(数据长度)，
字段名 数据类型(数据长度)
);
```

新建一张表，表名 `users`，包含 `u_id`、`u_name`、`u_phone` 三个字段：  

```sql
CREATE TABLE users (
u_id INT(10),
u_name CHAR(20),
u_phone CHAR(13)
);
```

再建立一张名为 `rooms` 的表，包含 20 个字符的 `r_name` 字段和 13 位字符的 `r_phone` 字段：  

```sql
CREATE TABLE rooms (
r_name CHAR(20),
r_phone CHAR(13)
);
```

现在使用 `SHOW tables;` 检查一下两张表是否建立成功。  

```sql
+-------------------+
| Tables_in_db_test |
+-------------------+
| rooms             |
| users             |
+-------------------+
```

## MySQL 常用的的数据类型

| 数据类型        | 字节       | 示例              | 说明                                                       |
|-----------------|------------|-------------------|------------------------------------------------------------|
| TINYINT         | 1          |                   | 2^8  = 255，带符号减半                                     |
| SMALLINT        | 2          |                   | 2^16 = 65536                                               |
| MEDIUMINT       | 3          |                   | 2^24 = 16777215                                            |
| INT             | 4          |                   | 2^32 Java 中的 int                                         |
| BIGINT          | 8          |                   | 2^64 Java 中的 long                                        |
| FLOAT           |            |                   | 单精度浮点数                                               |
| DOUBLE          |            |                   | 双精度浮点数                                               |
| DECIMAL NUMERIC |            | DECIMAL(4,1)      | 共4位，小数点后1位，小数点前3位                            |
| ENUM            |            | ENUM('a','b','c') | 枚举，单选                                                 |
| SET             |            | SET('1','2','3')  | 多选                                                       |
| DATE            | 3          | YYYY-MM-DD        | 日期                                                       |
| TIME            | 3          | HH:MM:SS          | 时间                                                       |
| DATETIME        | 8          |                   |                                                            |
| TIMESTAMP       | 4          |                   | 时间戳，一张表只能有一个时间戳列，时间戳能获取当前系统时间 |
| YEAR            |            | YYYY              | 年份                                                       |
| CHAR            | n*2        | CHAR(10)          | 定长字符串                                                 |
| VARCHAR         | 实际长度+1 | VARCHAR(50)       | 变长字符串，长度不超过255                                  |
| TEXT            |            |                   | 长文本，还有MEDIUMTEXT、LONGTEXT等，一般用TEXT就行         |
| BLOB            |            |                   | 长二进制数据，如图片、文档附件等                           |
| BIT             | 1          |                   | 只能存0或者1，最长64位                                     |

扩展阅读：[MySQL 中的数据类型介绍 - CSDN博客](https://blog.csdn.net/anxpp/article/details/51284106#comments)  

另外就是 INT BIGINT 这些类型也是可以指定长度的，例如 `INT(10)`，一般与
`ZEROFILL` 连用，用来在指定零填充位数。例如数据为1会存为 `0000000001`。

# 在数据表中插入数据

先查看 `users` 表，可以看到表内目前还没有数据：  

```sql
SELECT * FROM users;
```

插入数据的命令格式：  

```sql
INSERT INTO 表名(字段1,字段2,字段3) VALUES(值1,值2,值3);
```

例如插入以下内容到 `users`：  

| u_id | u_name  | u_phone |
|------|---------|---------|
| 1    | Alpha   | 54321   |
| 2    | Bravo   | 54322   |
| 3    | Charlie | 54323   |

```sql
INSERT INTO users(u_id,u_name,u_phone) VALUES(1,'Alpha','54321');
INSERT INTO users(u_id,u_name,u_phone) VALUES(2,'Bravo','54322');
INSERT INTO users(u_id,u_name,u_phone) VALUES(3,'Charlie','54323');

# 可以简写为：
INSERT INTO users(u_id,u_name,u_phone) VALUES
(1,'Alpha','54321'),
(2,'Bravo','54322'),
(3,'Charlie','54323');
```

再使用 `SELECT * FROM users;` 确认操作是否成功。  

```sql
+------|---------|---------+
| u_id | u_name  | u_phone |
+------|---------|---------+
|    1 | Alpha   | 54321   |
|    2 | Bravo   | 54322   |
|    3 | Charlie | 54323   |
+------|---------|---------+
```

# 创建 db_gamer 数据库

在 MySQL 中，我们可以通过使用 `source` 来引入一个包含 MySQL 代码的文件来操作数据库。  

举个例子，我们要新建一个数据库 `db_gamer`，这个数据库中有三张表。  

```sql
表 =rooms=，包含房间名称，房间内的游戏机：
+--------|-----------+
| r_room | r_console |
+--------|-----------+
| room1  | Xbox      |
| room2  | Switch    |
| room3  | PS4       |
| room3  | PSP       |
| room4  | Switch    |
+--------|-----------+

表 =users=，包含用户名，年龄，电话号码和所在房间：
+------|---------|-------|--------------|--------+
| u_id | u_name  | u_age | u_phone      | u_room |
+------|---------|-------|--------------|--------+
|    1 | Alpha   |    25 | 028-87654321 | room2  |
|    2 | Bravo   |    20 | 13888888888  | room2  |
|    3 | Charlie |    66 | 13777777777  | room3  |
|    4 | Delta   |    17 | 13666666666  | room1  |
|    5 | Echo    |    39 | 13555555555  | room4  |
+------|---------|-------|--------------|--------+

表 =games=，包含每个游戏机对应能玩的游戏，以及该游戏是谁买的：
+------|-----------|--------|---------+
| g_id | g_console | g_game | g_owner |
+------|-----------|--------|---------+
|    1 | XBox      | DMC4   |       2 |
|    2 | Switch    | Zelda  |       3 |
|    3 | Switch    | Mario  |       4 |
|    4 | PS4       | MHW    |       4 |
|    5 | PSP       | MH3G   |       1 |
+------|-----------|--------|---------+
```

如果手工逐步输入，有写到后面忘了前面的风险，对代码组织也不利。可以通过建立一个 `db_gamer.sql` 文件来一次性输入完整的代码，再导入 MySQL 即可。  

现在来创建这个文件：  

```shell
cd ~/
vim db_gamer.sql
```

将下列代码写入 `db_gamer.sql` 中：  

```myslq
# 创建数据库
CREATE DATABASE db_gamer;
USE db_gamer;

# 创建三张表
CREATE TABLE rooms (
  r_room CHAR(20) NOT NULL,
  r_console CHAR(20)
);

CREATE TABLE users (
  u_id INT PRIMARY KEY AUTO_INCREMENT,
  u_name CHAR(20) UNIQUE,
  u_age INT,
  u_phone CHAR(12) DEFAULT '028-87654321',
  u_room CHAR(20)
);

CREATE TABLE games (
  g_id INT PRIMARY KEY AUTO_INCREMENT,
  g_console CHAR(20),
  g_game CHAR(20),
  g_owner INT,
  FOREIGN KEY (g_owner) REFERENCES users(u_id)
);

# 插入数据
INSERT INTO rooms(r_room,r_console) VALUES('room1','Xbox');
INSERT INTO rooms(r_room,r_console) VALUES('room2','Switch');
INSERT INTO rooms(r_room,r_console) VALUES('room3','PS4');
INSERT INTO rooms(r_room,r_console) VALUES('room3','PSP');
INSERT INTO rooms(r_room,r_console) VALUES('room4','Switch');

INSERT INTO users(u_name,u_age,u_room) VALUES('Alpha',25,'room2');
INSERT INTO users(u_name,u_age,u_phone,u_room) VALUES('Bravo',20,'13888888888','room2');
INSERT INTO users(u_name,u_age,u_phone,u_room) VALUES('Charlie',66,'13777777777','room3');
INSERT INTO users(u_name,u_age,u_phone,u_room) VALUES('Delta',17,'13666666666','room1');
INSERT INTO users(u_name,u_age,u_phone,u_room) VALUES('Echo',39,'13555555555','room4');

INSERT INTO games(g_console,g_game,g_owner) VALUES('XBox','DMC4',2);
INSERT INTO games(g_console,g_game,g_owner) VALUES('Switch','Zelda',3);
INSERT INTO games(g_console,g_game,g_owner) VALUES('Switch','Mario',4);
INSERT INTO games(g_console,g_game,g_owner) VALUES('PS4','MHW',4);
INSERT INTO games(g_console,g_game,g_owner) VALUES('PSP','MH3G',1);
```

代码中有些不熟悉的内容，先不管它，进入 MySQL 并运行这个文件：  

```shell
mysql -r root
mysql> source ~/db_gamer.sql;
```

然后分别查看一下各表内容：  

```sql
SELECT * FROM rooms;
SELECT * FROM users;
SELECT * FROM games;
```

下面的学习都基于这个数据库。  

## 双减号和井号的区别
很多人不知道 `--` 和 `#` 的区别，写 SQL 文件这里可以很好的说明问题：

```sql
# 创建三张表 （这里可以用#号，因为这条语句到这里就结束了）
CREATE TABLE rooms (
  r_room CHAR(20) NOT NULL, -- 这里只能用双减号，因为实际下面两行代码仍然属于同一行 SQL 代码，用井号会把这条语句截断。
  r_console CHAR(20)
);
```

# SQL 约束 Constraint

使用 `cat -n ~/db_gamer.sql` 查看代码内容，`-n` 参数表示列印出行号，下面讲解为了方便会使用行号表述。  

代码中你所不熟悉的内容统统称之为**约束**，约束是一种限制，它对表的数据做出限制，来确数据的完整和表之间的关系。  

| PRIMARY KEY | FOREIGN KEY | DEFAULT | UNIQUE | NOT NULL | AUTO_INCREMENT |
|-------------|-------------|---------|--------|----------|----------------|
| 主键        | 外键        | 默认值  | 唯一   | 非空     | 自增           |

## 主键 (PRIMARY KEY)

第 12 行定义了主键  

主键用于约束表中的一行，作为这一行的唯一标识符，在一张表中通过主键就能准确定位到一行。  

主键不能有重复且不能为空。  

## 联合主键和复合主键
[联合主键和复合主键](https://blog.csdn.net/u011781521/article/details/71083112)

## 外键 (FOREIGN KEY)

第 24 行定义了外键  

外键主要体现表之间的关系，一个表可以有多个外键。  

每个外键必须 REFERENCES (参考) 另一个表的主键，被外键约束的列，取值必须在它参考的列中有对应值。  

在 `INSERT` 时，如果被外键约束的值没有在参考列中有对应，则操作失败。  

第 24 行意为 games 表中的 g\_owner 字段必须参考 users 表中的 u\_id 字段。  

## 默认值 (DEFAULT)

第 15 行定义了默认值  

有 `DEFAULT` 约束的字段，使用 `INSERT` 命令插入数据为空时，填入默认值。  

参考第 34 行，我们没有给用户 Alpha 写入 u\_phone，那么他的 u\_phone 就应该是 DEFAULT 定义的 "028-87654321"：  

```sql
ariaDB [db_gamer]> SELECT * FROM users;
+------|---------|-------|--------------|--------+
| u_id | u_name  | u_age | u_phone      | u_room |
+------|---------|-------|--------------|--------+
|    1 | Alpha   |    25 | 028-87654321 | room2  |
|    2 | Bravo   |    20 | 13888888888  | room2  |
|    3 | Charlie |    66 | 13777777777  | room3  |
|    4 | Delta   |    17 | 13666666666  | room1  |
|    5 | Echo    |    39 | 13555555555  | room4  |
+------|---------|-------|--------------|--------+
```

## 唯一约束 (UNIQUE)

第 13 行定义了唯一约束  

`UNIQUE` 约束的字段的值不能有重复。当 `INSERT` 数据重复时，会操作失败。  

和 `PRIMARY KEY` 相比，一张表可以有很多个唯一约束，但只能有一个主键。唯一约束允
许 null，但主键不行。

## 非空约束 (NOT NULL)

第 7 行定义了非空约束  

被非空约束的列，在插入值时必须非空。  

## 自增 (AUTO\_INCREMENT)

第 12、20 行定义了自增  

在第 34 ~ 38 行 `INSERT INTO users` 的时候，代码中并没有写入 `u_id` 的值，但表中自动为五个用户写入了 `u_id` 1、2、3、4、5，就是因为我们写入了自增约束  

第 40 ~ 44 行 `INSERT INTO games` 也是同理。

### 自增起始值

自增字段是可以从某一个号码开始的，比如QQ号最少五位，那么可以这么设计

```sql
CREATE TABLE user(
id BIGINT PRIMARY KEY AUTO_INCREMENT, -- QQ 号码
name VARCHAR(20), -- 网名
) AUTO_INCREMENT=10001; -- 定义起始值，第一个注册的用户是10001
```

也可以在设计完成后补录这个起始值

```sql
ALTER TABLE user SET AUTO_INCREMENT=10001;
```


# SELECT

## SELECT 基本语法

语法：  

```sql
SELECT 列名 FROM 表名 WHERE 条件;
```

例子：  

```sql
# 例1：查看 users 表内的所有内容：
SELECT * FROM users;

# 例2：查看 users 表内的用户和年龄：
SELECT u_name,u_age FROM users;

# 例3：通过 WHERE 查看 users 表内年龄大于 20 的用户：
SELECT u_name,u_age FROM users WHERE u_age>20;

# 例4：通过 WHERE 查看用户 Bravo 的年龄、电话：
SELECT u_name,u_age,u_phone FROM users WHERE u_name='Bravo';

# 例5：WHERE 后可以用 AND 和 OR：
SELECT u_name,u_age FROM users WHERE u_age<20 OR u_age>30;
SELECT u_name,u_age FROM users WHERE u_age>20 AND u_age<30;

# 例6：也可以用 BETWEEN：
SELECT u_name,u_age FROM users WHERE u_age BETWEEN 20 AND 30;
# 注意使用 BETWEEN 的时候是包含参数的，所以 20 岁的 Bravo 也被列了出来

# 例7：还可以用 IN 和 NOT IN 确定范围：
SELECT u_name,u_room FROM users WHERE u_room IN ('room1','room2');

# 例8：用 AS 可以更改字段的显示名称
SELECT u_name AS Username,u_room AS Roomname FROM users;
```
## 子查询
单行子查询非常简单：
```sql
# 找出哪些游戏是 Delta 买的：
SELECT g_game FROM games WHERE g_owner=(SELECT u_id FROM users WHERE u_name='Delta');
```

如果子查询返回的结果为多条数据，就称之为 `多行子查询` ，多行子查询需要在比较符和子查询语句之间插入关键字 `ALL` `SOME` `ANY`。

```sql
# ALL: 与子查询返回的列中的 所有 数值进行比较，如果比较结果为true，则返回。
SELECT * FROM users WHERE u_age > ALL(SELECT u_age FROM users WHERE u_name='Alpha' OR u_name='Delta');

+------+---------+-------+-------------+--------+
| u_id | u_name  | u_age | u_phone     | u_room |
+------+---------+-------+-------------+--------+
|    3 | Charlie |    66 | 13777777777 | room3  |
|    5 | Echo    |    39 | 13555555555 | room4  |
+------+---------+-------+-------------+--------+

# ANY: 与子查询返回的列中的 任一 数值进行比较，如果比较结果为true，则返回。
SELECT * FROM users WHERE u_age > ANY(SELECT u_age FROM users WHERE u_name='Alpha' OR u_name='Delta');

+------+---------+-------+--------------+--------+
| u_id | u_name  | u_age | u_phone      | u_room |
+------+---------+-------+--------------+--------+
|    1 | Alpha   |    25 | 028-87654321 | room2  |
|    2 | Bravo   |    20 | 13888888888  | room2  |
|    3 | Charlie |    66 | 13777777777  | room3  |
|    5 | Echo    |    39 | 13555555555  | room4  |
+------+---------+-------+--------------+--------+
```

另外 `SOME` 就是 `ANY` 的别名。

## LIKE + 通配符

| 通配符 | 说明              |
|--------|-------------------|
| `_`    | 表示一个字符      |
| `%`    | 表示0个或多个字符 |

例子：  

```sql
# 例1：查看所有 C 开头用户的电话号码：
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'C%';

# 例2：'%'的结果集中也包含"0个"
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'Charlie%';

# 例3：查看所有 B 开头并且名字为 5 位的用户的电话号码：
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'B____';
```

## DISTINCT
只列出不重复的值，例如要统计所有房间中总共有几种游戏机：
```sql
SELECT DISTINCT(g_console) FROM games;

+-----------+
| g_console |
+-----------+
| XBox      |
| Switch    |
| PS4       |
| PSP       |
+-----------+
```

## 排序 ORDER BY

| 排序方式 | 说明 |
|---- |--- |
| ASC  | 升序 |
| DESC | 降序 |

默认使用升序排列。  

```sql
# 例1：按年龄升序排序，这里用的默认，也可以加入 AES 明确
SELECT u_name,u_age FROM users ORDER BY u_age;

# 例1：按年龄降序排序
SELECT u_name,u_age FROM users ORDER BY u_age DESC;
```

## SQL 聚合函数

| 计数  | 求和 | 平均值 | 最大值 | 最小值 |
|-------|------|--------|--------|--------|
| COUNT | SUM  | AVG    | MAX    | MIN    |

例子：

```sql
# 例1：从 u_age 中取得最大最小两个值
SELECT MAX(u_age),MIN(u_age) FROM users;

# 例2：取得 users 表中年龄最大的用户的姓名和电话：
SELECT u_name,MAX(u_age),u_phone FROM users;

# 例3：计算有多少用户没有填写年龄：
SELECT COUNT(u_name)-COUNT(u_age) FROM users; -- 注意这里 COUNT 是对 u_age 计数，而不是对 u_age 的值进行计算

# 例4：计算用户年龄平均值：
SELECT AVG(u_age) FROM users;

# 例5：找出字母排序名字"最小"和"最大"的用户：
SELECT MIN(u_name),MAX(u_name) FROM users;
# MAX 和 MIN 可用于数字、字符串和日期时间数据类型
```

## 分组：GROUP BY

根据一个字段分组，SELECT后面只能跟分组字段和统计函数。

```sql
# 例1：计算每个 room 中有多少人
SELECT u_room,count(u_name) FROM users GROUP BY u_room;

# 例2：计算哪些房间里面有30岁以下的人，分别有多少个
SELECT u_room,count(u_name) FROM users WHERE u_age< 30 GROUP BY u_room; -- 如果要筛选，应该先筛选再分组
```

## HAVING 和 WHERE 的区别

问：Charlie 可以玩几种游戏机？

```sql
# 代码1：
SELECT r_room,COUNT(r_console) FROM rooms WHERE r_room=(SELECT u_room FROM usersWHERE u_name='Charlie') GROUP BY r_room;

# 代码2：
SELECT r_room,COUNT(r_console) FROM rooms GROUP BY r_room HAVING r_room IN (SELECT u_room FROM users WHERE u_name='Charlie');

+--------|------------------+
| r_room | COUNT(r_console) |
+--------|------------------+
| room3  |                2 |
+--------|------------------+
```

WHERE 是一个约束声明，使用 WHERE 约束来自数据库的数据，WHERE 是在结果返回之前起作用的，WHERE 中不能使用聚合函数。
WHERE 子句在查询过程中执行优先级高于聚合语句。

HAVING 必须在 GROUP BY 之后使用。
HAVING 是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在 HAVING 中可以使用聚合函数。
在查询过程中聚合语句 (SUM,MIN,MAX,AVG,COUNT) 要比 HAVING 语句优先执行。

参考资料：[Mysql中where与having的区别 - CSDN博客](https://blog.csdn.net/Mark_LQ/article/details/45012955)  
    
## 连接：JOIN ON

问：每个人分别能玩哪些游戏机？  

```sql
# 代码1：
SELECT u_name,r_console FROM users,rooms WHERE users.u_room = rooms.r_room;

# 代码2：
SELECT u_name,r_console FROM users JOIN rooms ON users.u_room = rooms.r_room;

+---------|-----------+
| u_name  | r_console |
+---------|-----------+
| Delta   | Xbox      |
| Alpha   | Switch    |
| Bravo   | Switch    |
| Charlie | PS4       |
| Charlie | PSP       |
| Echo    | Switch    |
+---------|-----------+
```

以上两行代码是等价的。代码2思路更清晰一些。  

使用 JOIN ON 也可以很方便的连接三张以上的表，例如要显示每个人分别能玩哪些游戏机和游戏：  

```sql
SELECT u_name,r_console,g_game FROM users JOIN rooms ON users.u_room = rooms.r_room JOIN games ON rooms.r_console = games.g_console;

+---------|-----------|--------+
| u_name  | r_console | g_game |
+---------|-----------|--------+
| Delta   | Xbox      | DMC4   |
| Alpha   | Switch    | Zelda  |
| Bravo   | Switch    | Zelda  |
| Echo    | Switch    | Zelda  |
| Alpha   | Switch    | Mario  |
| Bravo   | Switch    | Mario  |
| Echo    | Switch    | Mario  |
| Charlie | PS4       | MHW    |
| Charlie | PSP       | MH3G   |
+---------|-----------|--------+
```

也可以进行一些简单的计算，例如：  

要计数每个人分别在哪个房间能玩几种游戏，分别对姓名、房间、游戏数量进行重命名：  

```sql
SELECT u_name AS user,r_room AS which_room,COUNT(g_game) AS how_many_games FROM users JOIN rooms ON users.u_room = rooms.r_room JOIN games ON rooms.r_console = games.g_console GROUP BY u_name ORDER BY u_name;

+---------|------------|----------------+
| user    | which_room | how_many_games |
+---------|------------|----------------+
| Alpha   | room2      |              2 |
| Bravo   | room2      |              2 |
| Charlie | room3      |              2 |
| Delta   | room1      |              1 |
| Echo    | room4      |              2 |
+---------|------------|----------------+
```

# 函数
## 日期函数
```sql
# 当前时间
SELECT NOW();

# 获取某日期的年、月、日、时、分、秒
SELECT YEAR(NOW());
SELECT MONTH(NOW());
SELECT DAY(NOW());
SELECT HOUR(NOW());
SELECT MINUTE(NOW());
SELECT SECOND(NOW());

# 日期与 0000-00-00 之间的天数
SELECT TO_DAYS('2020-02-13');

# 日期之间相差的天数
SELECT DATEDIFF(now(), '2019-12-16');

# 计算从某日期开始之后的日期，INTERVAL后跟时间间隔
SELECT ADDDATE(NOW(), INTERVAL 1 YEAR);

# 计算从某日期开始之前的日期，INTERVAL后跟时间间隔
SELECT SUBDATE(NOW(), INTERVAL 1 YEAR);

# 获取当前时间戳（自 1970-01-01 00:00:00 到现在的毫秒数）
SELECT UNIX_TIMESTAMP();
```

## 格式化时间
函数 `DATE_FORMAT()` 接受两个参数，参数1为时间，参数二为格式化字符串。要注意的是
分钟是 `%i`，另外格式化字符串是区分大小写的。大写的情况下返回值会更完整一些。
```sql
# 全小写的情况
SELECT DATE_FORMAT(NOW(), '%y-%m-%d %h:%i:%s');
+-----------------------------------------+
| DATE_FORMAT(NOW(), '%y-%m-%d %h:%i:%s') |
+-----------------------------------------+
| 20-02-12 04:20:49                       |
+-----------------------------------------+

# 全大写的情况
SELECT DATE_FORMAT(NOW(), '%Y-%M-%D %H:%I:%S');
+-----------------------------------------+
| DATE_FORMAT(NOW(), '%Y-%M-%D %H:%I:%S') |
+-----------------------------------------+
| 2020-February-12th 16:04:11             |
+-----------------------------------------+
```
# todo in 和 exists
exists 修饰一个子查询，用在where之后，exists所在的表叫外表，子查询所在的表叫内表。
运作原理：查外表的所有符合条件的记录，然后依次遍历（loop）每条记录，该记录显示与
否取决于内表是否有记录集。
如果内表有记录，exists返回true，那么外表的那条记录就会显示。然后继续遍历下一条外
表记录。
区别在于 in 是子查询返回了一个集合，然后主查询去匹配
exists 是用外表的结果集去和内表返回的boolean做判断

为了性能考虑 EXISTS 用于外表记录数小于内表记录数的情况下。 
IN 则相反，用在子查询记录数小于主查询记录数的情况。

# todo UNION
把两条SQL的记录进行合并，一般用在数据统计上。
UNION 查出来的会去重
UNION ALL 不去重

# todo 更改记录
```sql
UPDATE users SET u_phone="12345" WHERE u_name='Alpha';
```

# doto 删除记录
```sql
DELETE FROM t where sname ='' 
```

# doto GRANT REVOKE
GRANT 授权，REVOKE 回收权限

授权 root 用户能够远程访问，密码是 root :
```sql
GRANT ALL PRIVILIEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root';
# 刷新权限，使其生效
FLUSH PRIVILIEGES;
```

其中两个 `*` ，第一个是库名，第二个是表名或者存储过程名等。

给 tom 所有 test 库的所有权限：
```sql
GRANT ALL ON test.* to tom identified by 'password';
```

授权用户 tom 对 test 库的 user 表 select insert update 的权限：
```sql
GRANT SELECT,INSERT,UPDATE ON test.user TO tom IDENTIFIED BY 'password';
```

回收 tom 在 test 库中的所有权限：
```sql
REVOKE ALL PRIVILEGES ON test.* FROM tom;
```




# 数据库备份
```sql
mysqldump -h 服务器 -P 端口号 -u 用户 -p 库名 > 文件

mysql -u root -p 库名 < 文件
```
