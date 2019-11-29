# tt

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

HTML 电报文本元素 (`<tt>`) 产生一个内联元素，使用浏览器内置的 monotype 字体展示。这个元素用于给文本排版，使其等宽展示，就像电报那样。使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code)元素来展示等宽文本可能更加普遍。



这个元素已废弃。使用更加适当的元素，例如带有 [CSS](https://developer.mozilla.org/fr/docs/CSS) 的[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code)或者[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)来代替。



## 属性

这个元素除了[全局属性](https://developer.mozilla.org/en-US/docs/Web/HTML/global_attributes)之外，没有其它属性，所有元素都一样。



## DOM接口

这个元素实现了[`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口。



**实现注解：** Gecko 1.9.2（包含）之前， Firefox 为这个元素实现了 [`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 示例

```javascript
<p>Enter the following at the telnet command prompt: <code>set localecho</code><br />

The telnet client should display: <tt>Local Echo is on</tt></p>
```

### 结果



在telnet命令提示符处输入以下内容： `set localecho`

telnet客户端会显示： `Local Echo is on`

## 注



- CSS 规范可以为 `tt` 选择器定义，来覆盖浏览器的默认字体。用于设置的偏好可能优先于指定的 CSS。



- 虽然这个元素没有在 HTML 4.01 规范中废弃，为了支持样式表，不建议使用它。



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)1  | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)1              | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18