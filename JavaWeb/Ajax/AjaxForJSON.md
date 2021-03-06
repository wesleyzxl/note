# 返回数据格式为XML

## JSON

- JSON（JavaScript Object  Notation）一种简单的数据格式，比xml更轻巧。JSON是JavaScript原生格式，这意味着在JavaScript中处理JSON数据不需要任何特殊的API或工具包。 
- JSON的规则很简单：对象是一个无序的“‘名称/值’对”集合。一个对象以“{”（左括号）开始，“}”（右括号）结束。每个“名称”后跟一个“:”（冒号）；“‘名称/值’对”之间使用“,”（逗号）分隔。

```jsp
<body>

<script type="text/javascript">
    var json = {
        "name":"wesley",
        "age":12,
        "address":{"city":"Shanghai", "road":"nanjing"},

        "play":function () {
            alert("games");
        }
    };

    alert(json.name);
    alert(json.address.city);

    json.play();
</script>
</body>
```

结果

![](pic/Snipaste_2019-03-19_20-26-26.png)

### 解析json

JSON只是一种文本字符串，它被存储在 responseText 属性中
为了读取存储在 responseText 属性中的 JSON 数据，需要根据 JavaScript 的 eval 语句。

具体请看例子

```json
{"person": {
  "name":"Andy Budd",
  "website":"http://andybudd.com/",
  "email":"andy@clearleft.com"
  }
}
```

接收页面添加js代码

```javascript
<script type="text/javascript">

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

                            // 获取
                            var result = request.responseText;
                            var object = eval("(" + result + ")");

                            var name = object.person.name;
                            var website = object.person.website;
                            var email = object.person.email;

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