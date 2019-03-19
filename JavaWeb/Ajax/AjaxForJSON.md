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