# command

- 贡献者1人

  

**已废弃**

此功能已过时。 虽然它可能仍然在某些浏览器中工作，但不鼓励使用它，因为它可能随时被删除。 尽量避免使用它。

[纠错](javascript:;)

**注意：**`command`元素已经被Gecko 24.0引擎移除以利于[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/menuitem)元素。Firefox从未支持`command`元素，并且在[Firefox 24](https://developer.mozilla.org/en-US/docs/Site_Compatibility_for_Firefox_24)中删除了对[`HTMLCommandElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCommandElement)DOM接口的实现。



该`command`元素表示用户可以调用的命令。

| 内容类别     | Flow content, phrasing content, metadata content.            |
| :----------- | :----------------------------------------------------------- |
| 允许的内容   | 否, 它是一个空元素                                           |
| 标签遗漏     | 必须有开始标签, 不可以有结束标签。                           |
| 允许的父元素 | 任何可以包含 [phrasing content](https://developer.mozilla.org/zh-cn/HTML/Content_categories#Phrasing_content)的元素 |
| DOM 接口     | HTMLCommandElement Obsolete since Gecko 24                   |

## 属性

该元素包括全局属性

**checked**表明该元素已被选择, 除非元素的`type`属性是`checkbox 或`者`radio`,否则该属性必须被省略.

**disabled**表明该command元素已经被禁用.

**icon**用一张图片来显示该command元素.

**label**该command元素的名称.用来显示给用户.

**radiogroup**如果该元素的`type`属性为`radio`,则radiogroup属性用来表示这一组command元素的公用名称. 如果`type`属性不是`radio`,则radiogroup属性必须省略.

**type**该属性用来表明command元素的类型,可以是下面三种值。

- `command` 或者清空，这是默认状态，并表示这是一个正常的命令。
- `checkbox` 表示该命令可以使用复选框进行切换。
- `radio` 表示该命令可以使用单选按钮切换。

## 示例

```javascript
<command type="command" label="Save" icon="icons/save.png" onclick="save()">
```

## 规范

| 规范                                            | 状态     | 评论 |
| :---------------------------------------------- | :------- | :--- |
| HTML Living Standard该规范中'<command>'的定义。 | 生活水平 |      |
| HTML5该规范中'<command>'的定义。                | 建议     |      |

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No1     | No                | No    | No     |
| checked       | No     | No   | No      | No                | No    | No     |
| disabled      | No     | No   | No      | No                | No    | No     |
| icon          | No     | No   | No      | No                | No    | No     |
| label         | No     | No   | No      | No                | No    | No     |
| radiogroup    | No     | No   | No      | No                | No    | No     |
| type          | No     | No   | No      | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No1                 | No        | No            | No         |
| checked       | No      | No                 | No          | No                  | No        | No            | No         |
| disabled      | No      | No                 | No          | No                  | No        | No            | No         |
| icon          | No      | No                 | No          | No                  | No        | No            | No         |
| label         | No      | No                 | No          | No                  | No        | No            | No         |
| radiogroup    | No      | No                 | No          | No                  | No        | No            | No         |
| type          | No      | No                 | No          | No                  | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18