title: MySQL 入门指南  
date: 2018-09-16  
updated:  
comments: true  
tags:  
-   learn
layout: post  
---

MySQL 语句是不区分大小写的，但是为了区分保留字和变量名，一般把保留字大写，变量和数据小写。  
本文是以前学习 MySQL 时写的，非常基础，内容也比一般的教程更详细一些，基本上操作完本文内容，就对数据库有了一个直观的了解了。  

<!-- more -->

# MySQL 的注释方法：

学习一门语言必须先学会注释，如果暂时看不懂下面内容，暂时掌握 `#` 注释即可。  

```mysql
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

# MySQL 数据库的基本使用方法

使用 root 登陆并查看本机数据库，注意此 root 并非 Linux 的 root，而是 MySQL 的 root，所以默认应该是没有密码的：  

```shell
mysql -u root
```

查看本机数据库：  

```mysql
mysql> SHOW databases;
```

连接至其中一个数据库：  

```mysql
mysql> USE 数据库名;
```

出现`database changeed`即表示连接成功  

查看数据库中的表：  

```mysql
mysql> SHOW tables;
```

退出 MySQL ：  

```mysql
mysql> EXIT
```

删除数据库：  

```mysql
mysql> DROP DATABASE 数据库名;
```

删除数据库中的表：  

```mysql
mysql> DROP TABLE 表名;
```

# 新建数据库和数据表

以下代码块中省略 `mysql>` 提示符。  

创建数据库和连接数据库的语法：  

```mysql
CREATE DATABASE 数据库名;
USE 数据库名;
```

实际操作创建并连接一个数据库 `db_test`：  

```mysql
CREATE DATABASE db_test;
USE db_test;
```

在数据库中新建一个表的语法：  

```mysql
CREATE TABLE 表名
(
字段名 数据类型(数据长度),
字段名 数据类型(数据长度)，
字段名 数据类型(数据长度)
);
```

新建一张表，表名 `users`，包含 `u_id`、`u_name`、`u_phone` 三个字段：  

```mysql
CREATE TABLE users (
u_id INT(10),
u_name CHAR(20),
u_phone CHAR(13)
);
```

再建立一张名为 `rooms` 的表，包含 20 个字符的 `r_name` 字段和 13 位字符的 `r_phone` 字段：  

```mysql
CREATE TABLE rooms (
r_name CHAR(20),
r_phone CHAR(13)
);
```

现在使用 `SHOW tables;` 检查一下两张表是否建立成功。  

```mysql
+-------------------+
| Tables_in_db_test |
+-------------------+
| rooms             |
| users             |
+-------------------+
```

## MySQL 常用的的数据类型

| 数据类型 | 示例              | 说明   |
|------- |----------------- |------ |
| INT     |                   | 整数   |
| FLOAT   |                   | 单精度浮点数 |
| DOUBLE  |                   | 双精度浮点数 |
| ENUM    | ENUM('a','b','c') | 单选   |
| SET     | SET('1','2','3')  | 多选   |
| DATE    | YYYY-MM-DD        | 日期   |
| TIME    | HH:MM:SS          | 时间   |
| YEAR    | YYYY              | 年份   |
| CHAR    |                   | 定长字符串 |
| VARCHAR |                   | 变长字符串 |
| TEXT    |                   | 长文本 |

扩展阅读：[MySQL 中的数据类型介绍 - CSDN博客](https://blog.csdn.net/anxpp/article/details/51284106#comments)  

# 在数据表中插入数据

先查看 `users` 表，可以看到表内目前还没有数据：  

```mysql
SELECT * FROM users;
```

插入数据的命令格式：  

```mysql
INSERT INTO 表名(字段1,字段2,字段3) VALUES(值1,值2,值3);
```

例如插入以下内容到 `users`：  

| u<sub>id</sub> | u<sub>name</sub> | u<sub>phone</sub> |
|-------------- |---------------- |----------------- |
| 1              | Alpha            | 54321             |
| 2              | Bravo            | 54322             |
| 3              | Charlie          | 54323             |

```mysql
INSERT INTO users(u_id,u_name,u_phone) VALUES(1,'Alpha','54321');
INSERT INTO users(u_id,u_name,u_phone) VALUES(2,'Bravo','54322');
INSERT INTO users(u_id,u_name,u_phone) VALUES(3,'Charlie','54323');
```

再使用 `SELECT * FROM users;` 确认操作是否成功。  

```mysql
+------|---------|---------+
| u_id | u_name  | u_phone |
+------|---------|---------+
|    1 | Alpha   | 54321   |
|    2 | Bravo   | 54322   |
|    3 | Charlie | 54323   |
+------|---------|---------+
```

# 创建 db\_<sub>gamer</sub> 数据库

在 MySQL 中，我们可以通过使用 `source` 来引入一个包含 MySQL 代码的文件来操作数据库。  

举个例子，我们要新建一个数据库 `db_gamer`，这个数据库中有三张表。  

```mysql
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

```mysql
SELECT * FROM rooms;
SELECT * FROM users;
SELECT * FROM games;
```

下面的学习都基于这个数据库。  

# SQL 约束

使用 `cat -n ~/db_gamer.sql` 查看代码内容，`-n` 参数表示列印出行号，下面讲解为了方便会使用行号表述。  

代码中你所不熟悉的内容统统称之为**约束**，约束是一种限制，它对表的数据做出限制，来确数据的完整和表之间的关系。  

| PRIMARY KEY | FOREIGN KEY | DEFAULT | UNIQUE | NOT NULL | AUTO<sub>INCREMENT</sub> |
|----------- |----------- |------- |------ |-------- |------------------------ |
| 主键        | 外键        | 默认值  | 唯一   | 非空     | 自增                     |

## 主键 (PRIMARY KEY)

第 12 行定义了主键  

主键用于约束表中的一行，作为这一行的唯一标识符，在一张表中通过主键就能准确定位到一行。  

主键不能有重复且不能为空。  

## 外键 (FOREIGN KEY)

第 24 行定义了外键  

外键主要体现表之间的关系，一个表可以有多个外键。  

每个外键必须 REFERENCES (参考) 另一个表的主键，被外键约束的列，取值必须在它参考的列中有对应值。  

在 `INSERT` 时，如果被外键约束的值没有在参考列中有对应，则操作失败。  

第 24 行意为 games 表中的 g<sub>owner</sub> 字段必须参考 users 表中的 u<sub>id</sub> 字段。  

## 默认值 (DEFAULT)

第 15 行定义了默认值  

有 `DEFAULT` 约束的字段，使用 `INSERT` 命令插入数据为空时，填入默认值。  

参考第 34 行，我们没有给用户 Alpha 写入 u<sub>phone</sub>，那么他的 u<sub>phone</sub> 就应该是 DEFAULT 定义的 "028-87654321"：  

```mysql
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

## 非空约束 (NOT NULL)

第 7 行定义了非空约束  

被非空约束的列，在插入值时必须非空。  

## 自增 (AUTO<sub>INCREMENT</sub>)

第 12、20 行定义了自增  

在第 34 ~ 38 行 `INSERT INTO users` 的时候，代码中并没有写入 `u_id` 的值，但表中自动为五个用户写入了 `u_id` 1、2、3、4、5，就是因为我们写入了自增约束  

第 40 ~ 44 行 `INSERT INTO games` 也是同理  

## TODO 约束实验

对于几种约束做几个实验：  

# SELECT

## SELECT 基本语法

语法：  

```mysql
SELECT 列名 FROM 表名 WHERE 条件;
```

例子：  

```mysql
# 例1：查看 users 表内的所有内容：
SELECT * FROM users;

# 例2：查看 users 表内的用户和年龄：
SELECT u_name,u_age FROM users;

# 例3：查看 users 表内年龄大于 20 的用户：
SELECT u_name,u_age FROM users WHERE u_age>20;

# 例4：查看用户 Bravo 的年龄、电话：
SELECT u_name,u_age,u_phone FROM users WHERE u_name='Bravo';

# 例5：WHERE 后可以用 AND 和 OR：
SELECT u_name,u_age FROM users WHERE u_age<20 or u_age>30;
SELECT u_name,u_age FROM users WHERE u_age>20 and u_age<30;

# 例6：也可以用 BETWEEB：
SELECT u_name,u_age FROM users WHERE u_age BETWEEN 20 AND 30;
# 注意使用 BETWEEN 的时候是包含参数的，所以 20 岁的 Bravo 也被列了出来

# 例7：使用 IN 和 NOT IN 确定范围：
SELECT u_name,u_room FROM users WHERE u_room IN ('room1','room2');

# 例8：找出哪些游戏是 Delta 买的：
SELECT g_game FROM games WHERE g_owner=(SELECT u_id FROM users WHERE u_name='Delta');
```

## LIKE + 通配符

| 通配符 | 说明      |
|--- |--------- |
| `_` | 表示一个字符 |
| `%` | 表示0个或多个字符 |

例子：  

```mysql
# 例1：查看所有 C 开头用户的电话号码：
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'C%';

# 例2：'%'的结果集中也包含"0个"
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'Charlie%';

# 例3：查看所有 B 开头并且名字为 5 位的用户的电话号码：
SELECT u_name,u_phone FROM users WHERE u_name LIKE 'B____';
```

## 排序 ORDER BY

| 排序方式 | 说明 |
|---- |--- |
| ASC  | 升序 |
| DESC | 降序 |

默认使用升序排列。  

```mysql
# 例1：按年龄升序排序，这里用的默认，也可以加入 AES 明确
SELECT u_name,u_age FROM users ORDER BY u_age;

# 例1：按年龄降序排序
SELECT u_name,u_age FROM users ORDER BY u_age DESC;
```

## SQL 聚合函数

| 计数  | 求和 | 平均值 | 最大值 | 最小值 |
|----- |--- |--- |--- |--- |
| COUNT | SUM | AVG | MAX | MIN |

例子：  

```mysql
# 例1：从 u_age 中取得最大最小两个值
SELECT MAX(u_age),MIN(u_age) FROM users;

# 例2：取得 u_age 中最大的值并重命名为 maxage，取得最小的值并重命名为 minage
SELECT MAX(u_age) AS maxage,MIN(u_age) AS minage FROM users;

# 例3：取得 users 表中年龄最大的用户的姓名和电话：
SELECT u_name,MAX(u_age),u_phone FROM users;

# 例4：计算有多少用户没有填写年龄：
SELECT COUNT(u_name)-COUNT(u_age) FROM users;
# 注意这里 COUNT 是对 u_age 计数，而不是对 u_age 的值进行计算

# 例5：计算用户年龄平均值：
SELECT AVG(u_age) FROM users;

# 例6：找出字母排序名字"最小"和"最大"的用户：
SELECT MIN(u_name),MAX(u_name) FROM users;
# MAX 和 MIN 可用于数字、字符串和日期时间数据类型
```

## 分组：GROUP BY

根据一个字段分组  

```mysql
# 例1：计算每个 room 中有多少人：
SELECT u_room,count(u_name) FROM users GROUP BY u_room;
```

## HAVING 和 WHERE 的区别

问：Charlie 可以玩几种游戏机？  

```mysql
# 代码1：
SELECT r_room,COUNT(r_console) FROM rooms WHERE r_room=(SELECT u_room FROM users WHERE u_name='Charlie');

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

HAVING 是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在 HAVING 中可以使用聚合函数。  
在查询过程中聚合语句 (SUM,MIN,MAX,AVG,COUNT) 要比 HAVING 语句优先执行。  

参考资料：[Mysql中where与having的区别 - CSDN博客](https://blog.csdn.net/Mark_LQ/article/details/45012955)  

## 连接：JOIN ON

问：每个人分别能玩哪些游戏机？  

```mysql
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

```mysql
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

```mysql
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
