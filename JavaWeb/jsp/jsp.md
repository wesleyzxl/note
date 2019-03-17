# JSP

## JPS注释

java server page

jsp中所使用的语法还是Java语言采用的语法形式

在JSP中有两类注释
  * 显式注释

    所有注释信息会发送的客服端，用户可以直接看见注释内容
    即HTML注释
    ```html
    <!-- 注释内容 -->
    ```

  * 隐式注释

    Java风格的注释，该类注释不会发送的客户端浏览器
    ```jsp
    <%
    // 注释

    /* 注释 */

    /**
    * 注释
    */
    %>
    ```

    JSP风格注释，该类注释也看不见
    ```jsp
    <%-- 注释内容 --%>
    ```

---

## Scriptlet

java中的一些名词
- applet 应用小程序
- servlet 服务器端小程序
- scriptlet 脚本小程序

脚本小程序是卸载JSP文件中的，主要的目的是编写java程序使用，同时可以有效地与html代码进行分离。

jsp中java代码需要与html代码进行分离

```jsp
<html>
...
<%
    // 这里可以编写程序逻辑
%>
...
</html>
```

*JSP代码一共定义有三类scriptlet代码*

1. <% %>

    ```java
    class c {
        /* field */
            // 常量
            // 属性（变量）
        /* Constructor */
        /* Method */
            // 局部变量
            // 程序语句
        /* 内部类 */
    }
    ```
    该Scriptlet主要功能是定义局部变量、编写程序的语句。相当于一个类中普通方法

    ```jsp
    <%
        int count = 0;
    %>

    <%
        //只要在一个页面中就是一个整体
        out.println(count ++);
    %>
    ```
    执行结果不管页面刷新多少次都是0

    **因为这个使用使用的局部变量，每一次执行都会重新声明**

2. <%! %>

    使用该种Scriptlet的主要作用可以定义全局常量、全局常量、方法、类（内部类）

    ```java
    <%!
        int count = 0;
        // 定义常量
        public static final String URL = "www.google.com"
    %>

    <%
        out.println(count ++);
        out.println(URL);
    %>
    ```
    count每次都会加一（重启浏览器效果也不会重置数值）

    此时count为全局变量，即使访问多次，也只会为count声明一次，不会重复声明。这样的全局变量很少会使用，使用的最多的是全局常量。

    *一般不在<%! %>中定义方法、类*

3. <%= %>

    可以进行表达式输出使用，可以直接输出内容

    ```jsp
    <%
        String str = "www.google.com";
        out.println("<h1>" + str + "</h1>");
    %>

    <h1><%=str%></h1>
    ```

    *使用第二种方式输出最大的好处在于，可以有效的将html代码与jsp中的变量输出进行分割*

    程序编写带来的一些困扰，与js代码容易混淆

    ```js
    <%
        String str = "www.google.com"
    %>

    <script type="text/javascript">
        alert("<%=str%>");
    </script>
    ```
    这里容易混淆的是alert函数中

    ```jsp
    <%
        String str = "www.google.com"
    %>

    <script type="text/javascript">
        alert(<%=str%>);
    </script>
    ```
    没有双引号是无法识别的。

    表达式输出比较容易出错，例如：通过jsp输出一个乘法口诀表

    ```jsp
    <html>
    <head>
        <title>$Title$</title>
    </head>
    <body>
    <table border="1">
        <%
            for (int i = 1; i <= 9; i++) {
        %>
        <tr>
            <%
                for (int j = 1; j <= i; j++) {
            %>
            <td><%=i%> * <%=j%> = <%=i * j%>
            </td>
            <%
                }
            %>
        </tr>
        <%
            }
        %>
    </table>
    </body>
    </html>
    ```
    输出结果
    ![Snipaste_2019-03-04_19-41-32](https://github.com/wesleyzxl/note/raw/master/JavaWeb/jsp/pic/Snipaste_2019-03-04_19-41-32.png)

---

## Page指令

在一个jsp文件里面，page指令是唯一一个描述jsp页面属性的指令，那么这个属性可能包括：**文件编码、MIME响应类型**、导入处理包、页面缓冲配置、session配置等等。

* 页面编码

    jsp中的编码分为两种：页面传输编码、页面显示编码。

    设置页面显示编码可以直接在page指令里面编写一个pageEcoding的属性信息。通常用编码UTF-8。

    ```jsp
    <%@ page pageEcoding="UTF-8"%>
    ...
    ```

    *注意@，并且页面的编码格式只能设置一种，不能设置多种*

    IDEA中的写法为

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    ```

* MIME响应类型

    MIME是页面最终的显示形式。参考：[MIME](http://www.cnblogs.com/jsean/articles/1610265.html)

* import指令

    导入操作的前提是该开发包必须在CLASSPATH之中。
    ```html
    <%@ page pageEcoding="UTF-8"%>
    <@ page import="java.util.*, java.text.*"%>
    ```

    import指令是唯一一个可以重复编写的page语句信息

    *在IDE中开发不需要导入*

---

## 连接Oracle数据库
要保证服务正常打开，同时需要将Oracle的驱动拷贝到TOMCAT_HOME/lib目录下（在使用记事本操作的情况下）

* 范例一

    编写程序连接Oracle，同时读取emp表的操作，将雇员的数据以表格的形式显示

```jsp
<%@ page import="java.sql.Connection" %>
<%@ page import="util.DBCPUtil" %>
<%@ page import="java.sql.PreparedStatement" %>
<%@ page import="java.sql.SQLException" %>
<%@ page import="java.sql.ResultSet" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    ResultSet rs = null;
    // 连接数据库
    Connection conn = DBCPUtil.getCon();
    String sql = "SELECT empno,ename, age, sal FROM employee";
    try {
        PreparedStatement pstmt = conn.prepareStatement(sql);
        rs = pstmt.executeQuery();
    } catch (SQLException e) {
        e.printStackTrace();
    }
%>

<table border="1" style="width: 100%" cellpadding="1" cellspacing="0" bgcolor="#F5F5F5">
    <thead>
    <tr bgcolor="#FFFFFF">
        <td>雇员编号</td>
        <td>姓名</td>
        <td>年龄</td>
        <td>薪水</td>
    </tr>
    </thead>

    <tbody>
    <%
        while (rs.next()) {
            int empno = rs.getInt(1);
            String ename = rs.getString(2);
            int age = rs.getInt(3);
            double sal = rs.getDouble(4);
    %>

    <tr bgcolor="#FFFFFF">
        <td><%=empno%></td>
        <td><%=ename%></td>
        <td><%=age%></td>
        <td><%=sal%></td>
    </tr>

    <%
        }
    %>
    </tbody>

</table>
</body>
</html>

```

...

* 范例二

    封装数据库连接，通过jsp输出表格，用MySQL数据库，通过Properties读取配置文件获取数据库连接池和jdbc配置信息

```java
package util;

import org.apache.commons.dbcp2.BasicDataSource;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class DBCPUtil {
    private static Connection con = null;

    private static BasicDataSource bdi = new BasicDataSource();

    static {
        Properties pp = new Properties();
        InputStream is = DBCPUtil.class.getClassLoader().getResourceAsStream("databaseConfig.properties");
        try {
            pp.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        String driver = pp.getProperty("driver");
        String url = pp.getProperty("url");
        String username = pp.getProperty("username");
        String password = pp.getProperty("password");
        int initialSize = Integer.parseInt(pp.getProperty("initialSize"));
        int maxIdle = Integer.parseInt(pp.getProperty("maxIdle"));
        int minIdle = Integer.parseInt(pp.getProperty("minIdle"));
        int maxWait = Integer.parseInt(pp.getProperty("maxWait"));
        int maxActive = Integer.parseInt(pp.getProperty("maxTotal"));

        bdi.setDriverClassName(driver);
        bdi.setUrl(url);
        bdi.setUsername(username);
        bdi.setPassword(password);
        bdi.setInitialSize(initialSize);
        bdi.setMaxTotal(maxActive);
        bdi.setMaxWaitMillis(maxWait);
        bdi.setMaxIdle(maxIdle);
        bdi.setMinIdle(minIdle);
    }

    public static Connection getCon() {
        try {
            con = bdi.getConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return con;
    }

    public static void closeCon() {
        try {
            if (con != null) {
                con.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

```jsp
<%@ page import="java.sql.Connection" %>
<%@ page import="util.DBCPUtil" %>
<%@ page import="java.sql.PreparedStatement" %>
<%@ page import="java.sql.SQLException" %>
<%@ page import="java.sql.ResultSet" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    ResultSet rs = null;
    // 连接数据库
    Connection conn = DBCPUtil.getCon();
    String sql = "SELECT empno,ename, age, sal FROM employee";
    try {
        PreparedStatement pstmt = conn.prepareStatement(sql);
        rs = pstmt.executeQuery();
    } catch (SQLException e) {
        e.printStackTrace();
    }
%>

<table border="1" style="width: 100%" cellpadding="1" cellspacing="0" bgcolor="#F5F5F5">
    <thead>
    <tr bgcolor="#FFFFFF">
        <td>雇员编号</td>
        <td>姓名</td>
        <td>年龄</td>
        <td>薪水</td>
    </tr>
    </thead>

    <tbody>
    <%
        while (rs.next()) {
            int empno = rs.getInt(1);
            String ename = rs.getString(2);
            int age = rs.getInt(3);
            double sal = rs.getDouble(4);
    %>

    <tr bgcolor="#FFFFFF">
        <td><%=empno%></td>
        <td><%=ename%></td>
        <td><%=age%></td>
        <td><%=sal%></td>
    </tr>

    <%
        }
    %>
    </tbody>

</table>
</body>
</html>
```

---

## include指令

在任何页面中，由一些固定部分组成

![Snipaste_2019-03-05_10-05-15](_v_images/20190311215143610_8053.png =654x)

为了更方便的重复使用代码，将这些部分的逻辑代码单独保存在文件之中，在需要的时候导入。

有两类语法实现

* 静态包含处理 <%@include file="path"%>

    path为需要导入文件的路径

* 动态包含处理 \<jsp:iclude page="path"/>

    与静态包含的最大区别为：动态包含会区分被包含的页面为静态页面还是动态页面，如果是动态页面则**按照先处理后包含的原则进行包含操作**，如果是静态页面，则与静态包含的处理方式相同。

    对于动态包含由于其指令形式为标签指令形式，那么该类指令在最后一定要有所完结，所以对于动态页面有以下两类

    * 不传递参数进行包含

        \<jsp:iclude page="path"/>

        此时效果将与静态包含效果相同

    * 传递参数进行包含

        ```jsp
        <jsp:include page="path">
            <jsp:param name="..." vlaue="..."/>
            ...
        </jsp:include>
        ```
        注意第二行代码**最后的/>**。所有这些参数在被包含页面都同意使用request.getParameter(String s)方法进行接收。

        include_demo.jsp
        ```jsp
        <%@ pageEncoding="UTF-8"%>
        <%
        String info = "hello world"
        // 传参需要使用表达式输出<%= %>
        %>
        <jsp:include page="param.jsp">
            <jsp:param name="paramA" value="hello"/>
            <jsp:param name="paramB" value="world"/>
            <jsp:param name="paramC" value="<%=info%>">
        </jsp:include>
        ```

        param.jsp
        ```jsp
        <%@ pageEncoding="UTF-8"%>
        <h1>参数A: <%=request.getParameter("paramA")%></h1>
        <h1>参数B: <%=request.getParameter("paramB")%></h1>
        <h1>参数C：<%=request.getParameter("paramC")%></h1>
        ```

        向标签指令被包含页面传递参数的时候一定要使用表达式输出。

* 两种包含的区别（面试题）


|    -    |         静态包含         |              动态包含               |
| ------- | ------------------------ | ---------------------------------- |
| 语法     | <%@include file="路径"%> | \<jsp:include page="路径"/>         |
| 处理流程 | 将代码包含在一起程序处理   | 先分别处理，将处理后的结果放在一起处理 |

基本都是使用动态包含

---

## forward指令

是一种无条件的跳转指令，专业称呼为转发。

语法：

1. 转发时不传递参数
    ```jsp
    <jsp:forward page="转发路径"/>
    ```

2. 转发传递参数
    ```jsp
    <jsp:forwward page="转发路径">
        <jsp:param name="参数名" value="参数内容"/>
        <jsp:param name="参数名2" value="参数内容">
        ...
    </jsp:forward>
    ```
路径不会发生改变，但是内容会变成跳转之后的内容。这种不改变请求路径的跳转在开发中被称为**服务器端跳转**，而改变请求路径的重定向成为**客户端跳转**。

跳转指令属于标签指令，操作的最后一定要完结标签，同时跳转属于服务器跳转操作。

## 用户登录案例

如果用户名密码正确进入欢迎页面，如果不符跳转到登录页面并且进行错误信息提示。

用户的信息需要保存在Oracle数据库里，需要自己创建一张用户表member，该表中只有两个字段mid(varchar2(50)), password(varchar2(32))，其中mid为主键

![Snipaste_2019-03-05_16-42-30](_v_images/20190311215314066_29949.png =630x)



每一个用户就是一个会话

---

## JSP中的九大隐含对象

内置对象不需要用户进行手工的实例化处理，会由WEB容器帮助用户提供此对象的实例

[**参考**](https://blog.csdn.net/kevin_love_it/article/details/60466260)

|   内置对象   |                 类型                  |             描述              |
| ----------- | ------------------------------------- | ----------------------------- |
| request     | javax.servlet.http.HttpServletRequest | 服务器端接收客服端发送的请求信息 |
| response    | javax.servlet.http.HttpServletRequest | 服务器端对客户端的回应信息      |
| application | javax.servlet.SeverletContext         | 描述整个应用服务的概念          |
| pageContext | javax.servlet.jsp.PageContext         | 描述的是一个JSP页面上下文信息   |
| config      | javax.servlet.ServletConfig           | 取得一些初始化的配置信息        |
| out         | javax.servlet.jsp.JspWriter           | 向客户端浏览器进行HTML输出      |
| page        | java.lang.Object                      | 描述的是一个JSP页面             |
| exception   | java.lang.Throwable                   | 每一个JSP页面都需要强制处理异常  |


![](_v_images/20190311221857227_9162.png =697x)



---

## 四种属性范围

对四种属性的特点和概念进行简单说明

属性范围值得是一个对象的保存范围。可能是只能在一个页面使用，也可能在整WEB服务器中使用，或只允许一个用户使用。

### 对象处理操作的三个方法

记住方法的名称、参数的类型及个数、返回值类型

1. 设置属性 public void setAttribute(String name, Object value);

2. 取得属性 public Object getAttribute(String name);

3. 删除属性 public void removeAttribute(String name);

拥有以上三个方法的对象一共有：pageContext, request, session, application

**注意：**

* pageContext有时候会简称为page，但是这和九个内置对象中的page不是同一个。

* 属性的保存的范围越大，其占用的内存就会越久。

### page属性范围

在一个页面设置的对象信息，只能够在本页面中取得，如果发生了重定向（跳转）则无法取得。

```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%
    // 设置属性
    pageContext.setAttribute("name", "N");
    pageContext.setAttribute("date", new Date();
%>

<%
    // 取得属性
    String myName = (String) pageContext.getAttribute("name");
    String d = (Date) pageContext.getAttribute("date");
%>

<h1>name 属性内容：<%=myName%></h1>
<h1>date属性内容：<%=d%></h1>
```
page属性占用内存时间最短

### request属性范围

如果希望发生了**服务器端跳转**（服务器端跳转不改变地址，客户端跳转改变地址）后，取得设置的属性值，可以使用request对象进行属性处理

a.jsp
```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%
    // 设置属性
    request.setAttribute("name", "N");
    request.setAttribute("date", new Date();
%>

<jsp:forward page="b.jsp"/>
```

b.jsp
```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%
    // 取得属性
    String myName = (String) request.getAttribute("name");
    String d = (Date) request.getAttribute("date");
%>
<h1>name 属性内容：<%=myName%></h1>
<h1>date属性内容：<%=d%></h1>
```

注意：
* 如果此时从b.jsp跳转到c.jsp之后（使用forward服务器跳转），该属性依然能狗保留下来
```jsp
<jsp:forward page="c.jsp"/>
```
* 如果使用超链接跳转，地址放生了改变，request属性将消失
```jsp
<a href="c.jsp">超链接</a>
```
* 因此，如果地址不发生改变，表示该请求将继续保留，则request的属性值将会保留；如果地址发生了改变，则反之。

### session属性范围

某一属性可以在服务器跳转和客户端跳转后都能够保存下来

a.jsp

```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>

<%
    // 设置属性
    session.setAttribute("name", "N");
    session.setAttribute("date", new Date();
%>

<jsp:forward page="b.jsp"/>
```

b.jsp 属性会保留
```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%
    // 取得属性
    String myName = (String) session.getAttribute("name");
    String d = (Date) session.getAttribute("date");
%>
<h1>name 属性内容：<%=myName%></h1>
<h1>date属性内容：<%=d%></h1>
<a href="c.jsp">超链接（改变地址、客户端跳转）</a>
```

c.jsp 属性依然保留
```jsp
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%
    // 取得属性
    String myName = (String) session.getAttribute("name");
    String d = (Date) session.getAttribute("date");
%>
<h1>name 属性内容：<%=myName%></h1>
<h1>date属性内容：<%=d%></h1>
```
只要是通过同一个浏览器设置过的session属性，中间不管经过多少次的跳转操作，一直到用户彻底关闭浏览器为止（不能只关闭页面）

session在实际的开发中主要的作用在于进行用户的登录检测，整个WEB开发，每个session对象保存的保存的都是一个用户（线程）信息，这些信息不是所有用户共享的。

### application属性范围

要设置的属性所有用户（线程）都可以看见，可以将该属性保存在属性保存在服务器上下文之中，通过application进行属性保存。

这个属性已经保存在了Tomcat的服务器之中，如果关闭了Tomcat服务器application属性就消失了。此属性保存范围是最长的，如果这个属性范围里面保存的对象数据过多，那么服务器的性能会下降。

### pageContext属性对象

javax.servlet.jsp.pageContext类，继承了父类javax.servlet.jsp.JspContext中两个方法：

* 设置属性

    public abstract void setAttribute(String name, Object value, int scope)
* 获得属性

    public abstract Object getAttribute(String name, int scope)
* 删除属性

    public abstract void removeAttribute(String name, int scope)

三个方法的参数列表中都有一个int scope变量，描述了属性的范围，在pageContext类中有四个常量来描述四种范围

* PAGE范围：public static final int PAGE_SCOPE
* REQUEST范围：public static final int REQUEST_SCOPE
* SESSION范围：public static final int SESSION_SCOPE
* APPLICATION范围：public static final int APPLICATION_SCOPE

所有属性范围都可以使用PageContext完成，但一般很少这样操作，而且pageContext只能用在JSP页面当中使用