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

需要先解析xml文件，构建对应的节点，放到目标的html文档对应的div中

需要转换为

```html
<h2><a href="mailto:andy@clearleft.com">Andy Budd</a></h2>
<a href="http://andybudd.com/">http://andybudd.com/</a>
```

于是js部分

```javascrip
	<script type="text/javascript">
            window.onload = function () {
                var aNodes = document.getElementsByTagName("a");
                for (var i = 0; aNodes.length > i; i++) {
                    aNodes[i].onclick = function () {
                        var request = new XMLHttpRequest();
                        var url = this.href;
                        var method = "GET";

                        request.open(method, url);
                        request.send(null);

                        request.onreadystatechange = function() {
                            if (request.readyState === 4) {
                                if (request.status === 302 || request.status === 200) {
                                    // 结果为xml格式，用responseXML来获取
                                    var result = request.responseXML;

                                    // 结果不能直接使用，必须先创建对应的节点，在把节点加入到对应div中
                                    var name = result.getElementsByTagName("name")[0].firstChild.nodeValue;
                                    var website = result.getElementsByTagName("website")[0].firstChild.nodeValue;
                                    var email = result.getElementsByTagName("email")[0].firstChild.nodeValue;

                                    // 生成html对应的格式
                                    var aNode = document.createElement("a");
                                    aNode.appendChild(document.createTextNode(name));
                                    aNode.href = "mailto" + email;
                                    var h2Node = document.createElement("h2");
                                    h2Node.appendChild(aNode);

                                    var aNode2 = document.createElement("a");
                                    aNode2.appendChild(document.createTextNode(website));
                                    aNode2.href = website;

                                    var detailsNode = document.getElementById("details");
                                    detailsNode.innerHTML = "";
                                    detailsNode.appendChild(h2Node);
                                    detailsNode.appendChild(aNode2);
                                }
                            }
                        };

                        return false;
                    };
                }
            };
        </script>
```


## 优缺点

### 优点：
- XML 是一种通用的数据格式。
- 不必把数据强加到已定义好的格式中，而是要为数据自定义合适的标记。
利用 DOM 可以完全掌控文档。
### 缺点：
如果文档来自于服务器，就必须得保证文档含有正确的首部信息。若文档类型不正确，那么 responseXML 的值将是空的。
当浏览器接收到长的 XML 文件后， DOM 解析可能会很复杂
