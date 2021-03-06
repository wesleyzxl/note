# Ajax

Ajax被认为是(Asynchronous JavaScript and XML的缩写）。现在，允许浏览器与服务器通信而无须刷新当前页面的技术都被叫做Ajax.

## 不用刷新整个页面便可与服务器通讯的办法：

- Flash
- Java applet
- 框架：如果使用一组框架构造了一个网页，可以只更新其中一个框架，而不必惊动整个页面
- 隐藏的iframe
- **XMLHttpRequest**：该对象是对 JavaScript 的一个扩展，可使网页与服务器进行通信。是创建 Ajax 应用的最佳选择。实际上**通常把 Ajax 当成 XMLHttpRequest 对象的代名词**

![Ajax的工作原理图](pic/Snipaste_2019-03-19_14-32-35.png)

## Ajax工具包

Ajax并不是一项新技术，它实际上是几种技术，每种技术各尽其职，以一种全新的方式聚合在一起
- 服务器端语言：服务器需要具备向浏览器发送特定信息的能力。Ajax与服务器端语言无关。
- XML (eXtensible Markup Language，可扩展标记语言) 是一种描述数据的格式。Aajx 程序需要某种格式化的格式来在服务器和客户端之间传递信息，XML 是其中的一种选择
- XHTML（eXtended Hypertext Markup Language,使用扩展超媒体标记语言）和 CSS（Cascading Style Sheet,级联样式单）标准化呈现；
- DOM（Document Object Model,文档对象模型）实现动态显示和交互；
使用XMLHTTP组件XMLHttpRequest对象进行异步数据读取
使用JavaScript绑定和处理所有数据

## XMLHttpRequest的概述

### XMLHttpRequest中的方法

|方法|描述|
|---|---|
|abort()|停止当前请求|
|getAllResponseHeaders()|把HTTP请求的所有相应首部作为键值对返回|
|getResponseHeader("header")|返回指定首部的串值|
|**open("method","url")**|建立对服务器的调用，Method参数可以是GET，POST etc. url可以是相对路径或者绝对路径|
|**send(content)**|向服务器发送请求|
|*setRequestHeader("header","value")*|把指定首部设置为所提供的值，在设置任何首部之前必须先调用open()|

### XMLHttpRequest的属性

|属性|描述|
|---|---|
|onreadystatechange|每个状态改变时都会触发这个事件处理器，通常会调用一个javaScript函数|
|readyState|请求的状态，5个可取值：0 未初始化，1 正在加载，2 已经加载，3 交互中，4 完成|
|responseText|服务器的响应，表示一个串|
|responseXML|服务器的相应，表示为XML。这个对象可以解析为DOM对象|
|status|服务器的HTTP状态码（200对应OK 304条件的GET 请求且该请求已被允许）|
|statusText|HTTP状态码的相应文本（OK或NotFound）|

## 案例

![](pic/Snipaste_2019-03-19_16-29-18.png)

```jsp
<%@ page pageEncoding="UTF-8" %>
<html>
<head>
    <title>Title</title>
</head>
<script charset="UTF-8" type="text/javascript">
    window.onload = function () {

        // 1. 获取a节点，添加onclick相应函数
        document.getElementsByTagName("a")[0].onclick = function () {

            // 3. 创建XMLHttpRequest对象
            var request = new XMLHttpRequest();

            // 4. 准备发送请求数据：url
            var url = this.href;
            var method = "get";

            // 5. 调用XMLHttpRequest对象的open方法
            request.open(method, url);

            // 6. 调用XMLHttpRequest对象的send方法
            request.send(null);

            /*
             * 7. 为XMLHttpRequest对象添加onreadystatechange相应函数，
             * 每个状态改变时都会触发这个事件处理器
             */
            request.onreadystatechange = function () {
                
                // 8. 判断相应是否完成（XMLHttpRequest对象的readyState属性为4时）
                if (request.readyState === 4) {
                    
                    // 9. 在判断相应值是否为200或304
                    if (request.status === 200 || request.readyState === 304) {

                        // 10. 打印相应结果responseTest
                        alert(request.responseText);
                    }
                    
                }
                
            };

            // 2. 取消a节点的默认行为
            return false;
        }
    }
</script>

<body>

<a href="hello.text">who?</a>

</body>
</html>
```

hello.text文件中保存了一句话：Hello Ajax!!

结果为![](pic/Snipaste_2019-03-19_16-32-14.png)


### 发送请求

open(method, url, asynch)
- XMLHttpRequest 对象的 open 方法允许程序员用一个Ajax调用向服务器发送请求。
- method：请求文件。可以是绝对路径或相对路径。类型，类似 “GET”或”POST”的字符串。若只想从服务器检索一个文件，而不需要发送任何数据，使用GET(可以在GET请求里通过附加在URL上的查询字符串来发送数据，不过数据大小限制为2000个字符)。若需要向服务器发送数据，用POST。
- 在某些情况下，有些浏览器会把多个XMLHttpRequest请求的结果缓存在同一个URL。如果对每个请求的响应不同，就会带来不好的结果。**在此将时间戳追加到URL的最后，就能确保URL的唯一性，从而避免浏览器缓存结果**。比如上面例子中可以改为var url = this.href + "?time=" + new Date();会更好
- url：路径字符串，指向你所请求的服务器上的那个
asynch：表示请求是否要异步传输，默认值为true。指定true，在读取后面的脚本之前，不需要等待服务器的相应。指定false，当脚本处理过程经过这点时，会停下来，一直等到Ajax请求执行完毕再继续执行。


onreadystatechange:
- 该事件处理函数由服务器触发，而不是用户
- 在 Ajax 执行过程中，服务器会通知客户端当前的通信状态。这依靠更新 XMLHttpRequest 对象的 readyState 来实现。改变 readyState 属性是服务器对客户端连接操作的一种方式。每次 readyState 属性的改变都会触发 readystatechange 事件


send(data)：
- open 方法定义了 Ajax 请求的一些细节。send 方法可为已经待命的请求发送指令
- data：将要传递给服务器的字符串。
若选用的是 GET 请求，则不会发送任何数据， 给 send 方法传递 null 即可：request.send(null);
- 当向send()方法提供参数时，要确保open()中指定的方法是POST，如果没有数据作为请求体的一部分发送，则使用null.
完整的 Ajax 的 GET 请求示例：
![](pic/pic2.png)


setRequestHeader(header,value)
- 当浏览器向服务器请求页面时，它会伴随这个请求发送一组首部信息。这些首部信息是一系列描述请求的元数据(metadata)。首部信息用来声明一个请求是 GET 还是 POST。
- Ajax 请求中，发送首部信息的工作可以由 setRequestHeader该完成
- 参数header： 首部的名字;  参数value：首部的值。
- 如果用 POST 向服务器发送数据，需要将 “Content-type” 的首部设置为 “application/x-www-form-urlencoded”.它会告知服务器正在发送数据，并且数据已经符合URL编码了。
- 该方法必须在open()之后才能调用
- 完整的 Ajax 的 POST 请求示例：
![](pic/pic3.png)

### 接收相应

readyState
- readyState 属性表示Ajax请求的当前状态。它的值用数字代表。
0 代表未初始化。 还没有调用 open 方法
1 代表正在加载。 open 方法已被调用，但 send 方法还没有被调用
2 代表已加载完毕。send 已被调用。请求已经开始
3 代表交互中。服务器正在发送响应
4 代表完成。响应发送完毕
- 每次 readyState 值的改变，都会触发 readystatechange 事件。如果把 onreadystatechange 事件处理函数赋给一个函数，那么每次 readyState 值的改变都会引发该函数的执行。
- readyState 值的变化会因浏览器的不同而有所差异。但是，当请求结束的时候，每个浏览器都会把 readyState 的值统一设为 4


status

- 服务器发送的每一个响应也都带有首部信息。三位数的状态码是服务器发送的响应中最重要的首部信息，并且属于超文本传输协议中的一部分。
- 常用状态码及其含义：
404 没找到页面(not found)
403 禁止访问(forbidden)
500 内部服务器出错(internal service error)
200 一切正常(ok)
304 没有被修改(not modified)
- 在 XMLHttpRequest 对象中，服务器发送的状态码都保存在 status 属性里。通过把这个值和 200 或 304 比较，可以确保服务器是否已发送了一个成功的响应


responseText

- XMLHttpRequest 的 responseText 属性包含了从服务器发送的数据。它是一个HTML,XML或普通文本，这取决于服务器发送的内容。
- 当 readyState 属性值变成 4 时, responseText 属性才可用，表明 Ajax 请求已经结束。


responseXML

- 如果服务器返回的是 XML， 那么数据将储存在 responseXML 属性中。
只用服务器发送了带有正确首部信息的数据时， responseXML 属性才是可用的。 MIME 类型必须为 text/xml


在开发中，需要用到原生的XMLHttpRequest的机会不多