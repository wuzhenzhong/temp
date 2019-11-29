# listing

已废弃

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

*HTML 列表元素* (`<listing>`) 渲染了开始和结束标签之间的文本，而不会解释 HTML，并使用等宽字体。HTML2 标准建议，当一行不超过 132 个字符时，不应该将其拆开。



注：不要使用这个元素。



- 它自从 HTML3.2 就废弃了，并且所有浏览器也不会实现它。它甚至在 HTML5 中过时，并且可能由某些浏览器渲染为 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 元素，它会解释其中的 HTML。



- 而是要使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 元素，或者如果满足语义的话，使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code) 元素，最终会转义 HTML '`<`' 和 `>`' ，以便它们不会被解释。



- 等宽字体也可以显示在 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div) 元素中，通过使用足够的 [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) 样式，在 [`font-family`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family) 中将 `monospace` 用作通用字体（generic-font）的值。



## 属性



除了 [全局属性](https://developer.mozilla.org/en-US/docs/Web/HTML/global_attributes) 之外，这个元素没有其它属性。



## DOM 接口



元素实现了 [`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口。



**实现注解：**直到 Gecko 1.9.2（包含），Firefox 为这个元素实现了[`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No1     | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No1                 | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18