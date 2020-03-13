# JSP 脚本
在 JSP 文件中可以定义三种脚本：
`<% 代码 %>`：
定义的 java 代码在对应的 servlet 的 service() 方法中，
也就是说 service 方法中可以定义什么，
该脚本就可以定义什么。

`<%! 代码 %>`：
定义的是对应的 servlet 的成员。
可以定义成员变量、成员方法、代码块等等。
一般用来定义成员方法。

`<%= 代码 %>`：
定义输出语句。转换为 out.print(代码);

`<%-- --%>`: 
JSP 的注释，也可以注释掉 html。

# JSP 指令
指令有三种：`page`、`include`、`taglib`

## `page` 设置页面的属性、导入模块等
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

* `contentType`: 等同于 `response.setContentType()`
* `import`: 导包
* `errorPage`: Servlet页面如果发生错误会 **转发** 到错误提示页面
* `isErrorPage`: 默认为 false，改为 true 才可使用 `exception` 这个内置对象

## `include` 在当前页面中包含另一个页面
```jsp
<%@include file="sub.jsp"%>
```

## `taglib` 引入标签库
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```
`prefix` 定义自定义前缀，一般是约定俗成的名称。

# JSP 内置对象
JSP 的内置对象是指在 JSP 中不需要获取和创建，可以直接使用的对象。

## JSP 的九个内置对象:
| 变量名      | 真实类型                               |
| ----------- | -------------------------------------- |
| pageContext | javax.servlet.jsp.PageContext          |
| request     | javax.servlet.http.HttpServletRequest  |
| respnse     | javax.servlet.http.HttpServletResponse |
| session     | javax.servlet.http.Session             |
| page        | java.lang.Object                       |
| application | javax.servlet.ServletContext           |
| config      | javax.servlet.SevletConfig             |
| exception   | java.lang.Throwable                    |
| out         | javax.servlet.jsp.JspWriter            |

### `pageContext`
### `request` 请求对象
### `response` 响应对象
### `session` 
### `page`
### `application`
### `config`
### `exception`:
只有在声明了 `isErrorPage="true"
### `out`：  
字符输出流对象，可以将数据输出到页面上。
和 response.getWriter() 的区别是，tomcat 永远会先识别 response.getWriter() 的缓冲区数据，再找 out.print() 的缓冲区数据。
也就是，response.getWriter() 的输出永远在 out.print() 之前。
尽量在 .jsp 页面中都使用 out 以免打乱输出顺序。

