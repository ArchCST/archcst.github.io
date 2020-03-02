# 字符串：StringBuilder StringBuffer String
StringBuilder 和 StringBuffer 都有殊API：增删改 char[]。

不可变字符串：被 final 所修饰，不可被继承，一量被赋值则不能修改
String.equals() 重写自 Object 方法，底层是 char[]数组的一一比较
String.indexOf() 找到相关字符串，返回索引

# 正则表达式：对字符串进行判断
```java
Pattern p = Pattern.compile("正则表达式规则");
Matcher matcher = p.matcher("要对比的字符串");
matcher.matches(); // 反回是否相等的 Boolean
```

# Calendar 日历类
Calendar 的 getInstance() 并不是单例模式。使用getInstance仅仅是因为Calendar类是一个抽象类无法直接实例化，所以 Calendar 类提供了一个获取子类对象的方法，这个方法名为getInstance()而已。在实际使用时实现特定的子类的对象。
由于Calendar类是抽象类，且Calendar类的构造方法是protected的，所以无法使用Calendar类的构造方法来创建对象，API中提供了getInstance方法用来创建对象。

# Object 类
加餐：深克隆
Object 类是所有类的超类，未使用extends关键字指明其基类，则默认为 extends Object