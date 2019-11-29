# acronym

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用它。

HTML Acronym  元素 (`<acronym>)`允许作者明确地声明一个字符序列,，它们构成一个单词的首字母缩写或简略语。



**用法提示:**. 该元素已从HTML5中移除，不应再被使用。Web开发者应使用[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/abbr)元素.



## 属性



该元素除了[global attributes](https://developer.mozilla.org/en-US/docs/HTML/global_attributes), 所有其他元素的公共属性之外没有其他属性.



## DOM 接口



该元素实现了[`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口.



**实现提示:**直到 Gecko 1.9.2（包含），Firefox 为这个元素实现了[`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 示例

```javascript
<p>The <acronym title="World Wide Web">WWW</acronym> is only a component of the Internet.</p>
```

## 默认样式



尽管这个标签的目的纯粹是为了方便作者，它的默认样式却在各个浏览器中不尽相同:



- 一些浏览器, 像Internet Explorer, 赋予它和 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span) 元素相同的样式。



- Opera, Firefox, 和 一些其他的浏览器在元素内容下方添加了一条点状的下划线。



- 一小部分浏览器不仅添加了点状下划线， 而且 put it in small caps; 为避免这种样式， 可以在 CSS 中添加[`font-variant`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant)`: none` 处理这种情况。



因此强烈建议Web作者们不要依赖默认的样式.



## 规范

| 规范                                     | 状态 | 评论 |
| :--------------------------------------- | :--- | :--- |
| HTML 4.01规范该规范中'<acronym>'的定义。 | 建议 |      |

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18