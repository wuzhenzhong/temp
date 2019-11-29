# plaintext

**已废弃**



此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

*HTML 纯文本元素* (`<plaintext>`) 将起始标签后面的任何东西渲染为纯文本，不会解释为 HTML。它没有闭合标签，因为任何后面的东西都会看做纯文本。



**注：**不要使用这个元素。



- 它自从 HTML3.2 就废弃了，并且所有浏览器也不会实现它。此外，它在 HTML5 中已过时；仍然接受它的浏览器，可能将其看做 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 元素，它仍然会解释其中的 HTML，即使这不是你想要的结果。



- 如果 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/plaintext) 元素是页面的第一个元素（除了任何不显示的元素），那就不要使用 HTML 了。配置你的服务器，使用 `text/plain` [MIME-type](https://developer.mozilla.org/en-US/docs/Properly_Configuring_Server_MIME_Types) 来发送你的页面。



- 不要使用这个元素，你应该使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 元素，或者如果满足语义的话，使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code) 元素。要确保转义了任何 '`<`' 、' `>`' 和 "&" 字符，来避免将内容解释为 HTML。



- 等宽字体也可以显示在 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div) 元素中，通过使用足够的 [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) 样式，在 [`font-family`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) 中将 `monospace` 用作通用字体（generic-font）的值。



## 属性

除了 [全局属性](https://developer.mozilla.org/en-US/docs/Web/HTML/global_attributes) 之外，这个元素没有其它属性。



## DOM接口

元素实现了 [`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement) 接口。



**实现注解：**直到 Gecko 1.9.2（包含），Firefox 为这个元素实现了 [`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement) 接口。



## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No1     | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No1                 | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18