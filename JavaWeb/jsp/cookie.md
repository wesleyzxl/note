# Cookie

指的是服务器存放到客户端的一些信息，记住密码的功能通过Cookie完成，但是安全性较差

Cookie类在java.servlet.http.Cookie，实现了Serializable, Cloneable

## Cookie类下有以下方法

```java
// 构造方法
public Cookie(String name, String value);

// 取得Cookie的名字
public String getName();

// 取得Cookie的值
public String getValue();

// 设置Cookie保存时间（以秒计时）
public void setMaxAge(int expiry);

// 设置保存路径
public void setPath(String uri);
```

## request, response下有关Cookie的方法
```java
/*
 * response
 * 在服务器端添加Cookie
 */
void addCookie(Cookie cookie)

/*
 * request
 * 获取全部的Cookie信息
 */
Cookie[] getCookie();
```
在chromm中F12的Application下显示cookie信息，cookie只在当前的浏览器有效

默认的设置是浏览器关闭则Cookie消失，且只在设置cookie的目录下的其他页面有效。可以用setMaxAge(int expiry)设置最大存活时间，用setPath(String uri)设置可访问目录。一般设置c.setPath(request.contextPath())，模块的根目录下
