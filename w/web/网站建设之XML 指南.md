# 网站建设之XML 指南

## XML 指南

------

## XML - 可扩展标记语言(EXtensible Markup Language)

XML 是跨平台的、用于传输信息且独立于软件和硬件的工具。

XML 文档实例

```
<?xml version="1.0"?>
<note>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>
```



------

## 什么是XML？

- XML 指*可扩展标记语言*（EXtensible Markup Language）
- XML 是一种*标记语言*，很类似 HTML
- XML 被设计用来*描述数据*
- XML 标签没有被预定义。您需要*自行定义标签*。
- XML 使用*文件类型声明*（DTD）或者 *XML Schema* 来描述数据。
- 带有 DTD 或者 XML Schema 的 XML 被设计为具有*自我描述性*。
- XML 是一个 W3C 标准

------

## XML不会做任何事情

ML是不做任何事情。 XML创建结构，存储和携带信息。

上面的XML文档的例子是XML编写的从Jani到Tove的一张纸条。注意标题和邮件正文。它还具有来自哪里的信息。但是，这个XML文档并没有做任何事情。只是纯粹的信息包裹在XML标记中。必须有人写了一款软件发送，接收或显示它：





------

## XML标签不是预定义

XML标签不是预定义，您必须"发明"自己的标签。

用来标记HTML文档的标签是预定义的的HTML文件作者只能使用在HTML标准（如<P>，<H1>等）定义的标签。

XML允许作者来定义他/她自己的标签和他/她自己的文档结构。

在上面的例子（像<to>和<from>）标签没有在任何XML标准定义。这些标签是XML文档作者"发明"的。

[查看一个XMLCD目录](https://www.w3cschool.cn/statics/demosource/cd_catalog.xml)

[查看一个XML植物目录](https://www.w3cschool.cn/statics/demosource/plant_catalog.xml)

[查看一个XML食品菜单](https://www.w3cschool.cn/statics/demosource/simple.xml)

## 如何学习XML？

[学习我们完整的XML教程](https://www.w3cschool.cn/xml/xml-tutorial.html)