- [MySQL 入门指南（二）](#sec-1)
  - [ALTER TABLE 对表进行修改](#sec-1-1)
    - [例：对 db<sub>gamer</sub> 进行修改](#sec-1-1-1)
  - [修改列可以改列名：](#sec-1-2)
    - [如何查询数据类型？](#sec-1-2-1)
  - [修改值](#sec-1-3)
  - [删除行](#sec-1-4)
  - [索引、视图、导入导出、备份恢复](#sec-1-5)
    - [索引](#sec-1-5-1)
    - [视图：](#sec-1-5-2)
    - [导入导出：](#sec-1-5-3)
    - [备份：](#sec-1-5-4)

# MySQL 入门指南（二）<a id="sec-1"></a>

## ALTER TABLE 对表进行修改<a id="sec-1-1"></a>

重命名表：

```mysql
ALTER TABLE 原名 RENAME TO 新名;
```

添加字段：

```mysql
ALTER TABLE 表名 ADD 列名 数据类型 约束;
```

添加字段默认添加到最末。如果要加入到中间，使用 `AFTER 列名` 即可，如果插入到第一列，使用 `FIRST`

删除字段：

```mysql
ALTER TABLE 表名字 DROP 列名字;
```

### TODO 例：对 db<sub>gamer</sub> 进行修改<a id="sec-1-1-1"></a>

```mysql
# 例1：将 rooms 表重命名为 homes
ALTER TABLE rooms RENAME TO homes;

# 例2：给 homes 添加 r_id 字段，并设定自增约束
```

## 修改列可以改列名：<a id="sec-1-2"></a>

### TODO 如何查询数据类型？<a id="sec-1-2-1"></a>

ALTER TABLE 表名字 CHANGE 原列名 新列名 数据类型 约束; 数据类型不可省略

修改列属性：

ALTER TABLE 表名字 MODIFY 列名字 新数据类型;

## 修改值<a id="sec-1-3"></a>

UPDATE 表名字 SET 列1=值1,列2=值2 WHERE 条件;

## 删除行<a id="sec-1-4"></a>

DELETE FROM 表名字 WHERE 条件; 不带 WHERE 就把表删掉了

## 索引、视图、导入导出、备份恢复<a id="sec-1-5"></a>

### 索引<a id="sec-1-5-1"></a>

CREATE INDEX 索引名 ON 表名字 (列名);

### 视图：<a id="sec-1-5-2"></a>

CREATE VIEW 视图名(列a,列b,列c) AS SELECT 列1,列2,列3 FROM 表名字;

### 导入导出：<a id="sec-1-5-3"></a>

导入导出 不适用于 MySQL 5.7.6 版本之后，如果需要尝试，请自行安装版本。 LOAD DATA INFILE '文件路径和文件名' INTO TABLE 表名字;

SELECT 列1，列2 INTO OUTFILE '文件路径和文件名' FROM 表名字;

LOAD DATA INFILE '文件路径' INTO TABLE pet LINES -> TERMINATED BY '\r\n';

windows 下换行符号需要声明，Mac OS 下需要使用 `\r` 来分行。

### 备份：<a id="sec-1-5-4"></a>

mysqldump -u root 数据库名>备份文件名; #备份整个数据库

mysqldump -u root 数据库名 表名字>备份文件名; #备份整个表

两种恢复备份的方法 source /tmp/SQL6/MySQL-06.sql mysql -u root test < bak.sql

取消命令：`\c`
