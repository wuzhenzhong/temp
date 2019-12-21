# 网站建设之JavaScript 指南

## JavaScript 指南

------

## JavaScript - 客户端脚本

JavaScript 是属于网络的脚本语言!

JavaScript 被数百万计的网页用来改进设计、验证表单、检测浏览器、创建cookies,以及更多的应用。

JavaScript 学习简单

JavaScript 实例

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>W3Cschool在线教程(w3cschool.cn)</title> 
<script>
function displayDate(){
	document.getElementById("demo").innerHTML=Date();
}
</script>
</head>
<body>

<h1>我的第一个 JavaScript 程序</h1>
<p id="demo">这是一个段落</p>

<button type="button" onclick="displayDate()">显示日期</button>

</body>
</html>
```



------

## 什么是 JavaScript?

- JavaScript 被设计用来向 HTML 页面添加交互行为。
- JavaScript 是一种脚本语言（脚本语言是一种轻量级的编程语言）。
- JavaScript 由数行可执行计算机代码组成。
- JavaScript 通常被直接嵌入 HTML 页面。
- JavaScript 是一种解释性语言（就是说，代码执行不进行预编译）。
- 所有的人无需购买许可证均可使用 JavaScript。

------

## 客户端脚本

JavaScript "制定" 浏览器行为。这就是所谓的客户端脚本（或浏览器的脚本）。

服务器端脚本是"制定"服务器的行为（见本站的ASP / PHP教程）。

------

## JavaScript可以做什么？

- **JavaScript 为 HTML 设计师提供了一种编程工具**
  HTML 创作者往往都不是程序员，但是 JavaScript 却是一种只拥有极其简单的语法的脚本语言！几乎每个人都有能力将短小的代码片断放入他们的 HTML 页面当中。
- **JavaScript 可以将动态的文本放入 HTML 页面**
  类似于这样的一段 JavaScript 声明可以将一段可变的文本放入 HTML 页面：document.write("<h1>" + name + "</h1>")
- **JavaScript 可以对事件作出响应**
  可以将 JavaScript 设置为当某事件发生时才会被执行，例如页面载入完成或者当用户点击某个 HTML 元素时。
- **JavaScript 可以读写 HTML 元素**
  JavaScript 可以读取及改变 HTML 元素的内容。
- **JavaScript 可被用来验证数据**
  在数据被提交到服务器之前，JavaScript 可被用来验证这些数据。
- **JavaScript 可被用来检测访问者的浏览器**
  JavaScript 可被用来检测访问者的浏览器，并根据所检测到的浏览器，为这个浏览器载入相应的页面。
- **JavaScript 可被用来创建 cookies**
  JavaScript 可被用来存储和取回位于访问者的计算机中的信息。

------

## 什么是HTML DOM？

HTML DOM 定义了访问和操作 HTML 文档的标准方法。

DOM 将 HTML 文档表达为树结构。

## HTML DOM Tree 实例

![DOM HTML tree](https://7n.w3cschool.cn/statics/images/course/htmltree.gif)





------

## 如何学习JavaScript？

[访问完整的 JavaScript 教程](https://www.w3cschool.cn/javascript/js-tutorial.html)

[访问 完整的 HTML DOM 教程](https://www.w3cschool.cn/htmldom/htmldom-tutorial.html)

[访问完整的 JavaScript 和 HTML DOM 参考手册](https://www.w3cschool.cn/jsref/jsref-tutorial.html)