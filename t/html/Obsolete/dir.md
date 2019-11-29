# dir

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但它的使用是不鼓励的，因为它可以在任何时候被删除。尽量避免使用它。

该 HTML 目录元素（`<dir>`）表示一个目录，即文件名的集合。

**使用说明：**请勿使用此元素。它存在于早期的 HTML 规范中，但在 HTML4 中已被弃用，然后在 [HTML5中 ](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)被废弃。改为使用`<ul>`。

## DOM 接口

这个元素实现了[`HTMLDirectoryElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDirectoryElement)接口。

## 属性

像所有其他HTML元素一样，此元素支持全局属性。

**compact** 这个布尔属性暗示列表应该以紧凑的风格呈现。该属性的解释取决于用户代理，并且不适用于所有浏览器。

**用法说明：**请勿使用此属性，因为它已被弃用：`<dir>`元素应使用[CSS](https://developer.mozilla.org/en-US/docs/CSS)进行样式设置。为了给出与该`compact`属性相似的效果，[CSS ](https://developer.mozilla.org/en-US/docs/CSS)属性[`line-height`](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)可以与值一起使用`80%`。

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No      | No                | No    | No     |
| compact       | No     | No   | No      | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No                  | No        | No            | No         |
| compact       | No      | No                 | No          | No                  | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18