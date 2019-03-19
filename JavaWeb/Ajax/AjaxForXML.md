# 返回数据格式为XML

xml文件中的书写方式

```xml
<?xml version="1.0" encoding="utf-8"?>
<details>
    <name>Andy Budd</name>
    <website>http://andybudd.com/</website>
    <email>andy@clearleft.com</email>
</details>
```

需要先解析xml文件，构建对应的节点，放到目标的html文档中

```javascript
window.onload = function () {
        var aNodes = document.getElementsByTagName("a");
        for (var i = 0; aNodes.length > i; i++) {
            aNodes[i].onclick = function () {

                var request = new XMLHttpRequest();
                var method = "GET";
                var url = this.href;

                request.open(method, url);
                request.send(null);

                request.onreadystatechange = function () {
                    if (request.readyState === 4) {
                        if (request.status === 302 || request.status === 200) {
                            document.getElementById("details").innerHTML = request.responseText;
                        }
                    }
                };

                return false;
            };
        }
    }
```