title: Java x D63 | MySQL
date: 2020-02-17
updated: 
comments: true
tags:
  - learn
  - MySQL
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
存储过程和触发器
{% endcq %}
<!--more-->


# 存储过程和触发器
题目：
根据现有的四张范例表，当录入某个学生某们课程的分数时，需要计算该分数是及格 、一
般或者优秀，然后更新到分数表的分数级别“scoreLevel”字段。（该字段自行设计）
条件如下： 分数 < 60 分，scoreLevel 为「不及格」
分数 >= 60 分 且 < 80 分，scoreLevel 为 「一般」 
分数 >= 80分，scoreLevel为 「优秀」
要求，计算过程用“存储过程”实现，结合触发器，贴出存储过程以及触发器的代码

第一种思路，用BEFORE触发器，调用存储过程来计算 scoreLevel 的值，然后把 NEW.scoreLevel 设置成存储过程的返回值，让触发器来完成整行数据的写入
```sql
# 为分数表添加字段
ALTER TABLE t_score ADD scoreLevel CHAR(3);

# 创建存储过程
DELIMITER //
DROP PROCEDURE IF EXISTS generateScoreLevel//
CREATE PROCEDURE generateScoreLevel(IN p_score DECIMAL(4,1), OUT p_sl VARCHAR(4))
BEGIN
IF p_score < 60 THEN
SET p_sl = '不及格';
ELSEIF p_score >= 60 AND p_score < 80 THEN
SET p_sl = '一般';
ELSE
SET p_sl = '优秀';
END IF;
END
//

# 创建触发器
DROP TRIGGER IF EXISTS insertScore//
CREATE TRIGGER insertScore BEFORE INSERT ON t_score FOR EACH ROW
BEGIN
DECLARE sl VARCHAR(4);
CALL generateScoreLevel(NEW.score, sl);
SET NEW.scoreLevel = sl;
END
//
DELIMITER ;
```

第二种思路，用 AFTER 触发器调用存储过程，因为是 AFTER，所以数据已经进入表中了，
就可以通过存储过程来 UPDATE scoreLevel。但因为我们的 t\_score 表没有 pk，所以第
二种方法无法实现，如果用 WHERE 去匹配，会有一些 BUG 解决起来很麻烦。
