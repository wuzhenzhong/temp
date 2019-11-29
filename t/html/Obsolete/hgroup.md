# hgroup

**这是一个实验中的功能**



此功能某些浏览器尚在开发中，请参考[浏览器兼容性表格](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/hgroup#Browser_compatibility)以得到在不同浏览器中适合使用的前缀。由于该功能对应的标准文档可能被重新修订，所以在未来版本的浏览器中该功能的语法和行为可能随之改变。



**HTML<hgroup>Element**(*HTML Headings Group Element*) 代表一个段的标题。它规定了在文档轮廓里（[the outline of the document](https://developer.mozilla.org/en-US/docs/Sections_and_Outlines_of_an_HTML5_document) ）的单一标题是它所属的隐式或显式部分的标题。



| 内容类别     | 流量内容，标题内容，可触及的内容。                |
| :----------- | :------------------------------------------------ |
| 允许的内容   | 一个或多个<h1>，<h2>，<h3>，<h4>，<h5>和/或<h6>。 |
| 标记遗漏     | 没有，起始和结束标签都是强制性的。                |
| 允许父母     | 任何接受流内容的元素。                            |
| 允许ARIA角色 | 标签，演示文稿                                    |
| DOM界面      | HTML元素                                          |

## 属性

这个元素仅包含全局属性。



## 使用说明

`<hgroup>`元素已从HTML5（W3C）规范中删除，但仍在WHATWG版本的HTML中。尽管在大多数浏览器中部分实现，所以不太可能消失。

但是，考虑到`<hgroup>`元素的关键目的是影响[HTML规范中定义的轮廓算法](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines#The_HTML5_outline_algorithm)如何显示标题- 但**HTML轮廓算法没有在任何浏览器中实现，**那么`<hgroup>`语义实际上只是理论上的。

因此，HTML5（W3C）规范提供了如何在不使用`<hgroup>`标记子[标题，字幕，替代标题和标语的建议](https://www.w3.org/TR/html5/common-idioms.html#sub-head)。

`<hgroup>`元素允许文档部分的主标题与任何次级标题（例如小标题或替代标题）分组，以形成*多级*标题。

换句话说，这个`<hgroup>`元素可以防止任何一个二级`<h1>–<h6>`孩子在大纲中创建他们自己的单独的部分 - 这些`<h1>–<h6>`元素通常会在没有任何子元素的情况下进行`<hgroup>`。

所以在[HTML规范中定义](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines#The_HTML5_outline_algorithm)的[HTML轮廓算法](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines#The_HTML5_outline_algorithm)产生的抽象轮廓中，`<hgroup>`整体形成一个单一的逻辑标题，整个`<h1>–<h6>`子`<hgroup>`轮廓作为一个*多级*单元进入轮廓，构成单一的抽象大纲中的逻辑标题。

为了产生这样一个大纲的任何（非抽象的）*呈现的*视图，必须在呈现工具的设计中做出关于如何呈现`<hgroup>`标题以便表达它们的多层次性质的选择。`<hgroup>`渲染后的轮廓可能有多种显示方式; 例如：

- 一个`<hgroup>`可能被示出为处于呈现的轮廓与冒号和空格（“ `:`”）或其他这样的标点会在主标题后

或所述第一次级标题之前（和具有相同或相似的标点符号任何其他辅助标题之前)

- 一个`<hgroup>`可能会显示在一个呈现的大纲中，主要标题之后是次要标题周围的括号，

思考下面的HTML文档：

```javascript
<!DOCTYPE html>
<title>HTML Standard</title>
<body>
  <hgroup id="document-title">
    <h1>HTML</h1>
    <h2>Living Standard — Last Updated 12 August 2016</h2>
  </hgroup>
  <p>Some intro to the document.</p>
  <h2>Table of contents</h2>
  <ol id=toc>...</ol>
  <h2>First section</h2>
  <p>Some intro to the first section.</p>
</body>
```

该文档的大纲应如下所示：

![img](https://ask.qcloudimg.com/http-save/devdocs/9sfk4cuei8.png)

也就是说，呈现的轮廓可能会显示主标题，*HTML*，后面跟冒号和空格，后面是辅助标题*Living Living - 2016年8月12日最后更新*。

或者，该文档的呈现大纲可能会改为如下所示：

![img](https://ask.qcloudimg.com/http-save/devdocs/vt54fexik6.png)

也就是说，呈现的大纲可能会显示主要标题*HTML*，后面是括号中显示的次要标题:( *生活标准 - 最近更新时间2016年8月12日）*。

## 示例

```javascript
<hgroup id="document-title">
  <h1>HTML</h1>
  <h2>Living Standard — Last Updated 12 August 2016</h2>
</hgroup>
```

| 规范                                   | 状态     | 评论 |
| :------------------------------------- | :------- | :--- |
| HTML生活标准该规范中'<hgroup>'的定义。 | 生活水平 |      |

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | 5      | (Yes) | 4       | 9                 | 11.1  | 4.1    |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | 2.2     | (Yes)              | (Yes)       | 4                   | 9         | 11            | 4.2        |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18