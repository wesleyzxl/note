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