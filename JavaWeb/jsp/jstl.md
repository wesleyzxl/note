# jstl

## 简单标签

需要将jstl和standard包放到tomcat的lib文件夹下


```java
List<Customer> list = new ArrayList<>();
list.add(cus1);
list.add(cus2);

request.setAttribute("list", list);
        request.getRequestDispatcher("tag.jsp").forward(request, response);
```

