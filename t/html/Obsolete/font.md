# font

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



*HTML Font 元素*（`<font>`）定义了该内容的字体大小、顏色与表现。



*注意：*



**不要使用这个元素！**尽管它在 HTML 3.2 规范化，但在 HTML 4.01 中已废除，因为该元件只和样式相关，接着在 HTML5 过时。



从 HTML 4 开始，HTML 不能在[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/style)元素，或各元素**style**属性以外，表现任何样式信息。今后的网页开发，样式只能使用[CSS](https://developer.mozilla.org/zh-TW/docs/CSS)来编写。



[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/font)元素的行为，可以通过[CSS](https://developer.mozilla.org/zh-TW/docs/CSS)属性实现，以及更好控制。



## 属性

如同其他 HTML 元素，这个元素支持[全局属性](https://developer.mozilla.org/en-US/docs/HTML/Global_attributes)。



**color**这个属性使用颜色名称或是十六进制的 #RRGGBB 格式，来设置文字的颜色。

**face**这个属性列出了一个或多个逗号分隔的字体名称。 默认样式中的文档文字，会使用客户端浏览器所支持的，第一个字体风格来渲染。如果本地系统中并没有安装列出的字体，浏览器会使用系统预设的均衡（proportional）或等宽（fixed-width）字体。

**size**这个属性使用数字或相对值指定文字大小。数字在最小的7到最大的7之间，默认值为3。也可以用诸如+2或-3的相对值指定，这会将其设置为，相对于[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/basefont)元素[`size`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/basefont#attr-size)属性的值，或者如果不存在，则是相对于3也就是默认值的值。



## DOM 接口

这个元素实现了[`HTMLFontElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFontElement)接口。



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| color         | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| face          | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| size          | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| color         | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| face          | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| size          | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18