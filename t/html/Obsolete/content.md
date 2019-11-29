# content

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



**HTML<content> 元素**— [Web 组件](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 的技术套件的废弃部分 — 用于 [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM) 内部作为 [insertion point](https://developer.mozilla.org/zh-CN/docs/Glossary/insertion_point)，并且不可用于任何正常的 HTML，现在已被[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/slot)元素代替，它在 DOM 中创建一个位置，Shadow DOM 会插入这里。



**注**：虽然在规范的草案中出现，并且在多个浏览器中实现，这个元素依然会在规范的之后版本中移除。



| 内容类别     | 透明的内容。                       |
| :----------- | :--------------------------------- |
| 允许的内容   | 流量内容。                         |
| 标记遗漏     | 没有，起始和结束标签都是强制性的。 |
| 允许的父元素 | 任何接受流内容的元素。             |
| DOM界面      | HTMLContentElement                 |

## 属性

这个元素包含 [全局属性](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)。



`select`逗号分隔的选择器列表，它们和 CSS 选择器语法相同。它们选择要插入的内容，来替换为 `<content> `元素。



## 示例

这里是一个使用`<content>`元素的简单示例。它是个 HTML 文件，包含所有所需的东西。



**注**：为了使这个代码有效，你使用的浏览器必须支持 Web 组件，请见 [Enabling Web Components in Firefox](https://developer.mozilla.org/en-US/docs/Web/Web_Components#Enabling_Web_Components_in_Firefox)。



```javascript
<html>
  <head></head>
  <body>
  <!-- The original content accessed by <content> -->
  <div>
    <h4>My Content Heading</h4>
    <p>My content text</p>
  </div>

  <script>
  // Get the <div> above.
  var myContent = document.querySelector('div');
  // Create a shadow DOM on the <div>
  var shadowroot = myContent.createShadowRoot();
  // Insert into the shadow DOM a new heading and 
  // part of the original content: the <p> tag.
  shadowroot.innerHTML =
   '<h2>Inserted Heading</h2> <content select="p"></content>';
  </script>

  </body>
</html>
```

如果你在 Web 浏览器中展示，它应该是这样。



![img](https://ask.qcloudimg.com/http-save/devdocs/a3r26s9hod.png)

## 规范

这个元素不再被任何规范定义。

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | 35     | No   | 331     | No                | 26    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | 37      | 37                 | No          | 331                 | No        | ?             | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18