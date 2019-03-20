# jstl

## 简单标签

需要将jstl和standard包放到tomcat的lib文件夹下

Servlet中request对象传值

```java
List<Customer> list = new ArrayList<>();
list.add(cus1);
list.add(cus2);

request.setAttribute("list", list);
        request.getRequestDispatcher("tag.jsp").forward(request, response);
```

jsp接收：tag.jsp

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<c:forEach items="${requestScope.list}" var="customer">
    - ${customer.age}, ${customer.name}<br>
</c:forEach>
```

tld文件在做支持

### 自定义标签

用户定义的一种自定义的jsp标记 。当一个含有自定义标签的jsp页面被jsp引擎编译成servlet时，tag标签被转化成了对一个称为 标签处理类 的对象的操作。于是，当jsp页面被jsp引擎转化为servlet后，实际上tag标签被转化为了对tag处理类的操作。 
![](pic/Snipaste_2019-03-20_13-18-16.png)

继承Simple