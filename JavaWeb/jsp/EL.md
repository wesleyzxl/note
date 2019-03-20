# EL

```jsp
<jsp:useBean id="customer" class:"com.entity.customer" scope="session"></jsp:useBean>
<jsp:setProperty property="age" value="12" name="customer"/>

<!--在本页面上可以直接获取到值-->
age:
<jsp:getProperty property="age" name="customer"/>

<!--但是如果是调转到另一个页面的时候上面的一行代码需要加上<jsp:useBean>的标签-->
```
但是使用${sessionScope.customer.age}就算跳转到另一个页面也可以得到age的值
同时使用${sessionScope.customer["age"]}也可以，这种情况适合在：
Servlet中写：

```java
Customer customer = new Customer();
customer.setName("zh");
session.setAttribute("com.entity.customer", customer);
```

在jsp中写
>name: ${sessionScope["com.entity.customer"].name}是可获取到值的
	
但是
>${sessionScope.com.entity.customer.name}是不可行的

所以在域对象的属性名中带特殊字符的时候需要用中括号写


## el变量

EL 存取变量数据的方法很简单，例如：${username}。它的意思是取出某一范围中名称为 username
的变量。因为我们并没有指定哪一个范围的 username，所以它的默认值会先从 Page 范围找，假如
找不到，再依序到 Request、Session、Application 范围。假如途中找到 username，就直接回传，
不再继续找下去，但是假如全部的范围都没有找到时，就回传 null

## el表达式可以进行自动的类型转换

${param.score + 11}这里进行的是数字的相加
<%=request.getParameter("score") + 11%>这里只是进行字符串的相加

## 与输入有关的隐含对象

param, paramValues

从Servlet重定向
```java
@WebServlet(name = "eltest", urlPatterns = "/eltest")
public...
	response.sendRedirect("eldemo.jsp?name=A&name=B&name=C");
```

在eldemo.jsp中接收

```jsp
${paramValues.name[0]}
```

这里获得的是一个数组，遍历需要使用jstl

## 在EL中可以一直使用对象中的getxxx方法

在Servlet中使用session传对象

```java
Date date = new Date();

HttpSession session = request.getSession();
session.setAttribute("date", date);
response.sendRedirect("eldemo.jsp?job=assassin&num=23");
```

于是在eldemo.jsp中

```jsp
${sessionScope.date.day}
```

这里相当于使用session.getAttribute("date");获得到Object类型，再强转为Date，在调用getDay()方法

## 其他隐含对象

pageContext etc.

### pageContext

```jsp
pageContext: ${pageContext.request.contextPath}
<br>

sessionId: ${pageContext.session.id}
<br>

sessionAttributeNames: ${pageContext.session.attributeName}
```

### cookie

```jsp
${cookie.JSESSIONID.name} = ${cookie.JSEESIONID.value}
```

### header


## EL的关系运算符

EL表达式的关系运算符要放在{}里面，放在外面无效

### 关系运算符


