title: SQLAlchemy 中外键的设置方法  
date: 2019-07-08  
updated:  
comments: true  
tags:  

-   cheatsheet

layout: post  

---


# 基本步骤1：创建外键

外键存储的是目标表的主键键值，外键总是在 **多** 的一侧定义。  

```python
class Article(db.Mode):
    ...
    author_id = db.Column(db.Integer, db.ForeignKey('author.id'))
```


# 基本步骤2：定义关系属性

关系属性在 **出发侧** 定义。相当于一个快捷查询，不会作为字段写入数据库。  

```python
class Author(db.Modle):
    ...
    articles = db.relationship('Article')
```


# 

|     | 出发侧 | 结束侧 |
| 一对多 | 定义关系 | 定义外键 |
| 多对一 | 定义 |      |
| 多对多 |      |      |
|     |      |      |
