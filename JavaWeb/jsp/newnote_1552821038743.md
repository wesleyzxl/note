## 转发

服务器跳转，地址栏地址不发生改变

* JSP中forward指令
    ```jsp
    // 不传参的forward指令
    <jsp:forward page="转发路径"/>

    // 传参的forward指令
    <jsp:forward page="转发路径">
        <jsp:param name="参数一名称" value="参数一"/>
        <jsp:param name="参数二名称" value="参数二">
    </jsp:forward>
    ```
    此时传参在另一处用request.getParmeter("参数名")接收参数

* RequestDispatcher getRequestDispatcher(String path)
  
    ServletRequest接口中的方法，此接口在javax.servlet包中

    RequestDispatcher接口中定义两个方法：

    1. void forward(ServletRequest request,ServletResponse response) throws ServletException, IOException
        
        用来传递request的，可以一个Servlet接收request请求，另一个Servlet用这个request请求来产生response。request传递的请求，response是客户端返回的信息。forward要在response到达客户端之前调用，也就是 before response body output has been flushed。如果不是的话，它会报出异常。


    2. void include(ServletRequest request,ServletResponse response) throws ServletException, IOException
        
        用来记录保留request和response，以后不能再修改response里表示状态的信息。
    
    可以通过ServletRequest接口中方法 void setAttribute(String name, Object o) 来stores an attribute in this request. 

    用Object getAttribute(String name)方法获取存在request对象中的attribute（Object对象）。atttibute中文为属性的意思

## 重定向
* response.sendRedirect(url);

* response.setState(302);

    response.setHeader("location",url);

## 总

1. request.getRequestDispatcher()是请求转发，前后页面共享一个request ; 

    response.sendRedirect()是重新定向，前后页面不是一个request。

2. RequestDispatcher.forward()是在服务器端运行; 
   
    HttpServletResponse.sendRedirect()是通过向客户浏览器发送命令来完成. 

    所以RequestDispatcher.forward()对于浏览器来说是“透明的”； 而HttpServletResponse.sendRedirect()则不是。

3. ServletContext.getRequestDispatcher(String url)中的url只能使用绝对路径； 

    ServletRequest.getRequestDispatcher(String url)中的url可以使用相对路径。
    
    因为ServletRequest具有相对路径的概念；而ServletContext对象无此概念。RequestDispatcher对象从客户端获取请求request，并把它们传递给服务器上的servlet,html或jsp。