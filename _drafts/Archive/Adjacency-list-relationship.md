title: SQLAlchemy 中邻接列表关系的理解  
date: 2019-07-19  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

最近在看李辉老师的 Flask Web 开发实战，对「邻接列表关系」有一些理解。  

[源码](https://github.com/greyli/bluelog/blob/master/bluelog/models.py) 中 `Comment` 类实现了可以对评论进行回复的功能，关键是在定义「邻接列表关系」这里，这三行代码我的理解如下：  

```python
# comment.replied_id 为一个指向评论（父级，也是自身）的外键
replied_id = db.Column(db.Integer, db.ForeignKey('comment.id'))

# comment.replies 为同一评论下的所有“回复”（子级，因为「多」所以变量名采用了复数形式）
replies = db.relationship('Comment', back_populates='replied', cascade='all, delete-orphan')

# comment.replied 为被回复的评论（父级，也是「一」这一侧的关系定义）
replied = db.relationship('Comment', back_populates='replies', remote_side=[id])
```

之所以有第三行（replied）是「一」这一侧必须要有关系定义，从而SQLAlchemy才能到对应的「多」侧去找外键字段（replied<sub>id</sub>)。  

`comment.replied_id` 对回复调用才有返回值，返回该回复对应的评论的 id。（返回父级 id）  
`comment.replies` 对评论调用才有返回值，返回该评论对应的回复对象构成的列表。（返回子级对象集合）  
`comment.replied` 对回复调用才有返回值，返回该回复对应的评论对象。（返回父级对象）  

在 `fakes.py` 中创建虚拟回复数据时，就是把 `Comment（子级/回复）` 对象的 `replied` 属性设置为某个随机的 `Comment（父级/评论）` 对象：  

```python
for i in range(salt):
    comment = Comment(
        ...
        replied=Comment.query.get(random.randint(1, Comment.query.count())),
        ...
    )
```
