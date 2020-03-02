title: Java x D58 | MySQL
date: 2020-02-12
updated: 
comments: true
tags:
  - learn
  - MySQL
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
MySQL 查询语句练习
{% endcq %}
<!--more-->

# 数据库创建
下面的练习基于这个数据库：
```sql
-- 创建学生表
#drop table if EXISTS t_student;
create table t_student(
sno char(10) primary key,
sname varchar(20) not null,
sex char(1) not null,
birthday date,
home varchar(20),
blood enum('A','B','AB','O','-')
);

-- 创建课程表
#drop table if EXISTS t_course;
create table t_course(
cno char(10) primary key,
cname varchar(20) not null,
tno char(10) not null
);

-- 创建分数表
#drop table if EXISTS t_score;
create table t_score(
sno char(10) not null,
cno char(10) not null,
score decimal(4,1) not null
);
-- 创建教师表
# drop table if EXISTS t_teacher;
create table t_teacher(
tno char(10) primary key,
tname varchar(20),
sex char(1) not null,
birthday date,
prof varchar(10),
department varchar(20)
);

# 添加学生数据
insert into t_student values('108','曾华','男','1977-09-01','浙江杭州','A');
insert into t_student values('105','匡明','男','1975-10-02','浙江杭州','B');
insert into t_student values('107','王丽','女','1976-01-23','湖北武汉','A');
insert into t_student values('101','李军','男','1976-02-20','湖北武汉','A');
insert into t_student values('109','王芳','女','1975-02-10','四川成都','B');
insert into t_student values('103','陆君','男','1974-06-03','四川成都','B');

# 添加讲师数据
insert into t_teacher values('804','李诚','男','1958-12-02','副教授','计算机系');
insert into t_teacher values('856','张旭','男','1969-03-12','讲师','电子工程系');
insert into t_teacher values('825','王萍','女','1972-05-05','助教','计算机系');
insert into t_teacher values('831','刘冰','女','1977-08-14','助教','电子工程系');

# 添加课程信息
insert into t_course values('3-105','计算机导论','825');
insert into t_course values('3-245','操作系统','804');
insert into t_course values('6-166','数字电路','856');
insert into t_course values('9-888','高等数学','831');

# 添加成绩信息
insert into t_score values('103','3-245','86');
insert into t_score values('105','3-245','75');
insert into t_score values('109','3-245','68');
insert into t_score values('103','3-105','92');
insert into t_score values('105','3-105','88');
insert into t_score values('109','3-105','76');
insert into t_score values('103','6-166','85');
insert into t_score values('105','6-166','79');
insert into t_score values('109','6-166','81');
```

# 习题
## 查询没有参加 `cno='3-105'` 的学生有哪些？
```sql
SELECT sname FROM t_student WHERE sno NOT IN (SELECT sno FROM t_score WHERE cno='3-105');
```

用 `EXISTS` 来做：
```sql
SELECT sname FROM t_student stu WHERE NOT EXISTS(SELECT 1 FROM t_score WHERE sno=stu.sno AND cno='3-105');
```

## todo 查询参加 `cno='3-105'` 这门考试的最高分和最低分，以及这些学生的名字
我的答案，只会显示一条最高分和一条最低分，也就是说两个人同时考了100只会列出其中
一人。
```sql
(SELECT sname,score FROM t_student JOIN t_score ON t_student.sno=t_score.sno 
WHERE cno='3-105' ORDER BY score DESC LIMIT 1) 
UNION
(SELECT sname,score FROM t_student JOIN t_score ON t_student.sno=t_score.sno 
WHERE cno='3-105' ORDER BY score ASC LIMIT 1);
```

小幸幸的答案，同分会都显示出来：
```sql
SELECT t_student.sname,t_score.score FROM t_score,t_student
WHERE t_student.sno=t_score.sno AND t_score.score
IN (SELECT MAX(t_score.score) FROM t_score
WHERE t_score.cno='3-105'
UNION
SELECT MIN(t_score.score) FROM t_score
WHERE t_score.cno='3-105');
```


## 查询所有课程的平均分，显示所有课程的名称、课程编号和平均分
我的答案，用了虚拟表
```sql
SELECT cname, t_course.cno, avgscore FROM t_course JOIN 
(SELECT cno,AVG(score) avgscore FROM t_score GROUP BY cno) AS a ON t_course.cno=a.cno;
```

小幸幸的答案，不用虚拟表
```sql
SELECT
t_course.cno,
t_course.cname,
AVG(t_score.score) AS average
FROM
t_course,t_score
WHERE t_score.cno=t_course.cno
GROUP BY t_course.cno;
```

## 计算每个人的总成绩并排名(要求显示字段:姓名，总成绩)
```sql
SELECT sname,SUM(score) FROM t_student JOIN t_score ON t_student.sno=t_score.sno 
GROUP BY sname ORDER BY SUM(score) DESC;
```
## 计算每个人单科的最高成绩(姓名、科目、分数)
```sql
SELECT stu.sname,cou.cname,n.ms FROM t_score t 
JOIN (SELECT sno,MAX(score) ms FROM t_score GROUP BY sno) as n ON t.sno=n.sno AND t.score=n.ms
JOIN t_student stu ON t.sno=stu.sno
JOIN t_course cou ON t.cno = cou.cno;
```
## 计算每个人的平均成绩
```sql
SELECT sname,AVG(score) FROM t_student JOIN t_score ON t_student.sno=t_score.sno GROUP BY sname;
```

## 列出某们课程成绩最好的学生
```sql
SELECT cou.cname,stu.sname FROM t_score sco 
JOIN (SELECT cno,MAX(score) ms FROM t_score GROUP BY cno) AS n ON sco.cno=n.cno AND sco.score=n.ms 
JOIN t_student stu ON sco.sno=stu.sno
JOIN t_course cou ON n.cno=cou.cno;
```
