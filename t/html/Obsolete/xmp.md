# xmp

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

<xmp>标签之间的内容不会被当作文档内容解析，而会被用等宽字体直接呈现。HTML2规范建议，本标签中的内容应该具有足够容纳每行80个字母的宽度。



**提示:**请不要使用这个元素



- 从HTML3.2开始反对使用本元素，同时它不再会在之后版本内被推荐使用。在HTML5规范内，本元素已经完全被移除。



- 建议您使用[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 元素，如果有特殊需求，您可以使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code) 元素。需要您注意的是，使用元素的时候，需要将内容中的"<"转义为"&lt;"，以防止被浏览器当作正常标签解析。



- 通过[CSS](https://developer.mozilla.org/en-US/docs/CSS)样式表将 [`font-family`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) 属性的值设置成为generic-font，可以让任何其他任何标签获得等宽字体的样式。



## 属性

该元素支持[全局属性](https://developer.mozilla.org/en-US/docs/Web/HTML/global_attributes)。



## DOM接口



本元素支持[`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口。



**兼容性提示:**Gecko 1.9.2内核及更高版本已经兼容本元素，在火狐浏览器内本元素支持使用[`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)1  | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)1              | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18