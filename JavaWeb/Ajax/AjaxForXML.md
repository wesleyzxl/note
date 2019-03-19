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

