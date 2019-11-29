# applet

**已废弃**



[纠错](javascript:;)

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用它。

HTML中的Applet元素(`<applet>`) 标志着包含了Java的applet。





**注解：** 这个元素在HTML5中已经废弃了，不能再被使用。网页开发者应该使用更为通用的元素。

## 属性（Attributes）



**align**该属性被用来定义网页上applet相对于周围内容的对齐方式。HTML4.01规范定义了bottom, left, middle, right和 top的值，而Microsoft（微软）和Netscaple（网景）也可能支持**absbottom**, **absmiddle**, **baseline**, **center**和**texttop。**

**alt**该属性造成在不支持Java的浏览器上显示出一段替代的描述性文字。 页面设计者应该记住在`<applet>`元素中封闭的内容也可以呈现为替代性文本。

**archive**该属性涉及到applet的存档或压缩版本及其相关类文件，这可能会减少下载时间。

**code**该属性指定被加载和执行的applet类文件的URL。Applet文件名由一个.class文件扩展名定义。由code指定的URL可能与codebase属性有关。

**codebase**该属性给出绝对或相对的，由code属性引用的applet的.class文件储存的目录的URL。**datafld**该属性支持Internet Explorer4及更高的版本，指定提供边界数据的数据源对象的列名。该属性可以用来指定各种传递到Java的[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/param)元素。

**datasrc**就像datafld，该属性被用于Internet Explorer 4版本以下的数据绑定（data binding）。它指明了数据源对象的id，这个数据对象提供了被与applet有关的元素约束的数据。

**height**该属性提供了applet所需的高度，以像素为单位。

**hspace**该属性指定了保存在applet两侧的额外横向空间，以像素为单位。

**mayscript**在Netscape中，该属性允许使用在文档中嵌入的脚本语言程序访问applet。

**name**该属性为applet分配一个名称，以便它可以被其他资源识别，尤其是脚本语言。

**object**该属性指定一个序列化表示的applet的URL。

**src**为Internet Explorer 4 及更高版本制定, 该属性为applet相关文件指定一个URL。 该定义及使用是不明确的，也不属于HTML标准。

**vspace**该属性指定了保存在applet以上或以下的额外垂直空间，以像素为单位。**width**该属性指定了applet所需的宽度，以像素为单位。



## 示例

```javascript
<applet code="game.class" align="left" archive="game.zip" height="250" width="350">
  <param name="difficulty" value="easy">
  <b>Sorry, you need Java to play this game.</b>
</applet>
```

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No1     | (Yes)             | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No1                 | (Yes)     | No            | No         |

## Notes

The W3C specification does not encourage the use of `<applet>` and prefers the use of the `<object>` tag. Under the strict definition of HTML 4.01, this element is deprecated and entirely obsolete in HTML5.

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18