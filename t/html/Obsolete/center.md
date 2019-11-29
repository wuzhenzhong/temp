# center

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



[纠错](javascript:;)

HTML Center 元素 (`<center>`) 是个[块级元素](https://developer.mozilla.org/en-US/docs/HTML/Block-level_elements)，可以包含段落，以及其它块级和内联元素。这个元素的整个内容在它的上级元素中水平居中(通常是[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body))。



这个标签已经在 HTML 4（以及 XHTML 1）中废除了，以支持[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)[`text-align`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align)属性，它可以用于 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div)元素，或者独立的[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p)。对于居中的块，使用其它 CSS 属性，例如 [`margin-left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-left)和[`margin-right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-right)，并将其设置为 `auto`(或者将 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin)设为`0 auto`).



## DOM 接口



这个元素实现了[`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口。



**实现注解:** 直到 Gecko 1.9.2（包含）, Firefox 为这个元素实现了 [`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 示例1

```javascript
<center>This text will be centered.
<p>So will this paragraph.</p></center>
```

## 示例 2 (CSS 替代)



```javascript
<div style="text-align:center">This text will be centered.
<p>So will this paragraph.</p></div>
```

## 示例 3 (CSS 替代)



```javascript
<p style="text-align:center">This line will be centered.<br>
And so will this line.</p>
```

## 注



向[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div)或者 {HTMLElement("p")}} 元素应用 {Cssxref("text-align")}}`:center`会使这些元素的*内容*居中，当它们的外形尺寸没有改变的时候。



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)1  | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)1              | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18