## load()方法

- load() 方法是 jQuery 中最为简单和常用的 Ajax 方法, 能载入远程的 HTML 代码并插入到 DOM 中. 
- 它的结构是:   load(url[, data][,callback])

将用原生Ajax写的导入html改写

```jsp
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <title>People at Clearleft</title>
    <style type="text/css">
        @import url("clearleft.css");
    </style>
    <script type="text/javascript" src="../jsDemo/jquery-3.3.1.js"></script>
    <script type="text/javascript">

        $(function () {
            $("a").click(function () {
                // 使用load方法处理Ajax
                // 这里的url是DOM对象
                var url = this.href;
                var args = {"time":new Date()};

                $("#details").load(url, args);

                return false;
            });
        });

    </script>
</head>
<body>
<h1>People</h1>
<ul>
    <li>
        <a href="files/andy.html">Andy</a>
    </li>
    <li>
        <a href="files/richard.html">Richard</a>
    </li>
    <li>
        <a href="files/jeremy.html">Jeremy</a>
    </li>
</ul>

<div id="details"></div>
</body>
</html>
```

