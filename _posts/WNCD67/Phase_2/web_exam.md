title: Java x D80 | Servlet
date: 2020-03-05
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
Web 理论周考 A 卷
{% endcq %}
<!--more-->

# 一、JSP的内置对象有哪些，作用是什么，至少四个以上
request：向服务器发送请求的对象
response：响应，将处理过的信息回传到客户端
out：输出内容到页面
application：ServletContext
Session：保存会话信息，可以保存 Object，在超时前该信息在多个页面之间有效。

# 二、实现Servlet由哪几种方式？
实现 Servlet 接口
继承 HttpServlet 抽象类

# 三、HttpServlet类当中的doGet、doPost、sevice有什么区别
service 处理所有客户端的请求，HttpServlet 重新了 service 方法，其中判断了请求的方法，将处理方法重新封装成了 doGet 和 doPost 方法。
doGet 处理 GET 方式的请求。
doPost 处理 POST 方式的请求。

# 四、Servlet中的init、destroy和service方法分别在何时执行，执行次数有什么特点
init 默认在页面第一被访问时执行，在 servlet 生命周期中只会执行一次。
如果希望在服务器启动时执行，可以在`web.xml`中配置`<servlet><load-on-startup>1</load-on-startup></servlet>`。
destroy 在被正常关闭服务器之前执行，只会执行一次。
service 可以在整个 servlet 的生命周期中多次执行，处理每一个请求。

# 五、Statement、PreparedStatement、CallableSatement、ResultSet接口有什么作用
ResultSet 存储查询结果的结果集

# 六、如何给一个Servlet配置多个访问路径，写出其配置代码
`@WebServlet({"/path1","/path2"})`

# 七、如何在web.xml中给Servlet传递启动参数，写出其配置代码

# 八、Servlet何时被实例化？可以配置吗
默认情况下是第一次访问时才会被实例化。可以通过 `load-on-startup` 配置为服务器启动时实例化。

# 九、get和post方法有什么区别，超链接什么什么请求类型？表单提交默认是什么请求类型？如果提交的表单内容比较多，用什么请求类型
get 请求的内容会暴露在地址栏，根据浏览器不同，长度限制不同。
post 请求的内容会放在请求体中，相对更安全一点，容量理论上无限。
超链接是 get 请求，表单默认提交也是 get 请求，可以通过 `method="post"` 设置请求类型。

# 十、HTTP状态码说出三个以上
1xx：
2xx：正常
3xx：正常，包含301 永久重定向，302临时重定向，304缓存
4xx：资源访问不到
5xx：服务器报错

# 十一、浏览器提示404错误，原因是
资源找不到，很可能是路径配置错误。

# 十二、如何设置web应用的首页或者叫欢迎页
index.html
index.jsp

# 十三、获取表单中单选按钮的值，比如性别，用什么代码
```java
request.getParameter("sex");
```

# 十四、获取表单中隐藏域的值，用什么代码
```java
request.getParameter("隐藏域的name");
```

# 十五、获取表单中一组复选框叫name=”abc”的值，用什么代码

# 十六、request.getParameterValues()和request.getParameter()有什么区别
`request.getParameter("key")` 获取一个字符串。
`request.getParameterValues("key")` 获取一个字符串数组，通常用来获取复选框的多个值。

# 十七、使用response进行重定向时，用什么代码
```java
response.sendRedirect("url");
```

# 十八、使用request进行请求转发时，用什么代码
```java
request.getRequestDispatcher("url").forward(request, response);
```

# 十九、request.seAttribute(“属性名”, 值) 在重定向到新页面的时候，可以把值传给新页面吗？
request.sendRedirect()
不在同一个 request 域，无法通过 request 把值传给新页面。

# 二十、用response.sendRedirect()重定向到新页面，能传递String和数字吗？能传递集合类这种数据类型吗？
可以通过 `查询字符串` 传递 String 和数字，其实都是 String。不能传递集合类。
二十一、把用户名保存在session对象中的代码是？在JSTL中的代码是？
```
session.setAttribute("属性名", Object);
<c:set var="" value="" scope="session" />
```


# 二十二、哪个内置对象可以从浏览器开启到关闭之间跨多个请求之间持续有效？
session

# 二十三、哪个内置对象可以从应用服务器启动到关闭之间跨多个请求之间持续有效？
ServletContext

# 二十四、session的sessionID是做什么的？何时产生的？存在哪里？和登录有关系吗？
用来区分客户端。
第一请求会生成一个 JSESSIONID，Cookie

# 二十五、session有效期几种设置方式

# 二十六、临时Cookie对象和持久cookie对象分别保存在哪里？对应的关键代码是？
临时 cookie 存放在浏览器内存。
持久 cookie 存放在硬盘。
Cookie.setMaxAge(int second);

# 二十七、如果要使一个过滤器过滤/my/*开头的请求，在web.xml中的配置代码是
```xml
<filter-mapping>
    <filter-name>accessControl</filter-name>
    <url-pattern>/my/*</url-pattern>
</filter-mapping>
```

# 二十八、不同浏览器窗口的session能共享吗？
可以

# 二十九、MVC流程简要说明，请求是发给哪一层？


# 三十、useBean相当于java什么代码？
```
<jsp:userBean class="me.archcst.entity.User" id="user">
```

相当于 Servlet 中：
```
User user = new User();
```

# 三十一、JSTL迭代一个集合类List<某个Bean>的代码是
```jsp
```


# 三十二、如何解决post请求发过来的中文乱码
# 三十三、如何解决服务器端响应到客户端页面的中文乱码
# 三十四、如何在服务器端给前端发送json对象，关键代码
```java
response.setContentType("text/json;charset=utf-8");
response.getWriter().print("Json字符串");
```

# 三十五、前端$.get $.post $.ajax三个方法有什么区别
# 三十六、如果要通过 `$.ajax` 发送一个异步post请求，传递如下数据 `name=tom&email=tom@qq.com&action=reg` 请写出 `$.ajax` 的代码
```js
$.ajax({
    type: "post",
    url: "路径",
    data: "name=tom&email=tom@qq.com&action=reg",
    success: function (msg) {};
});
```
# 三十七、前端jquery如何获取表单输入框id=”email”的值
```js
$('#email')
```

# 三十八、前端jquery如何获取表单一组复选框 name=”intrests” 的值并弹出内容
```js
$('#email')
```

# 三十九、新建一个动态web工程，jar包放在哪里？JSP页面和静态资源放在哪里？
```
/WEB-INF/lib
```

# 四十、运行tomcat web工程，构建路径必须要包含哪两个库？
JDK 和 Tomcat

# 四十一、jquery的代码写在哪里，写出代码轮廓
```html
<script src="/path/to/jquery.js"></script>
<script>
$(function () {
    alert('Hello world!');
})
</script>
```

# 四十二、ServletConfig参数有什么用？
获取 web.xml 配置的启动参数。

# 四十三、解决POST请求的中文乱码有几种方式？
request.setCharacterEncoding("utf-8");

# 四十四、前端ajax请求发送一个json对象到后端可以吗？写出其语法
```js
$.get("url", {}, function (params) {})
```

# 四十五、前端ajax请求发到后端，后端返回一个普通text/plain内容和返回一个text/json内容，对于前端有什么区别？
通知前端返回的数据类型。 `text/json`

# 四十六、前端ajax请求发到后端JSP，如果JSP包含有HTML，该如何解决返回给前端的数据不包含页面HTML标签?
在 HTML 标签之前加入 return 语句。
