# basefont

**已废弃**



此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用.

HTML标签<basefont>用来设置文档的默认字体大小。使用[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/font)可以相对于默认字体大小进行变化。



*使用说明:*



**不要再使用这个标签！** 尽管在HTML 3.2中曾经（不严格地）标准化，但是它并不被主流的浏览器所支持。而且，不同的浏览器、甚至同一浏览器的相邻版本，都没有使用相同的实现方式； 实际上，使用这个标签总是导致不确定的结果。



<basefont>元素，同其他只与样式相关的元素一起，在标准中不被建议使用。从HTML 4起，HTML不再传递样式信息（除[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/style)元素和所有元素的**style**属性内容外）。在HTML5，这个元素已经被彻底移除。对于所有新的网页开发，样式只应该写在CSS中。



使用[CSS Fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Fonts)属性，同样能够实现[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/font)的效果，甚至更好控制。



## 属性

如同其他HTML元素一样，它支持[全局属性](https://developer.mozilla.org/en-US/docs/HTML/Global_attributes).



**color**该属性使用颜色名称或者形如#RRGGBB的十六进制格式设置字体的颜色。

**face**该属性包含一个或多个字体名称。文档文字默认按照第一个浏览器支持的字体进行渲染。如果所有列出的字体本地系统都未安装，浏览器默认使用该系统上的定比或者定宽字体。

**size**该属性定义了字体大小的，使用数值或者相对值。数值值域为1～7，1最小，默认值为3。



## DOM接口



该元素实现了[`HTMLBaseFontElement`](https://developer.mozilla.org/en-US/docs/DOM/HTMLBaseFontElement)`接口`.



## 示例

```javascript
<basefont color="#FF0000" face="Helvetica" size="+2" />
```

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | (Yes) | No      | (Yes)             | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | (Yes)       | No                  | (Yes)     | No            | No         |

## Notes

- HTML 3.2支持basefont元素，但只支持size属性。

- 严格的HTML和XHTML规范不支持这个元素。

- 尽管是过渡标准的一部分，但一些标准化的浏览器（如Mozilla和Opera）不支持这一元素。

- 这个元素可以用元素上的CSS规则来模仿`<body>`。

- XHTML 1.0需要这个元素的尾部斜线：`<basefont />`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18