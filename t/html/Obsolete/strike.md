# strike

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

**HTML<strike> 元素**（或者 HTML 删除线元素）在文本上放置删除线。



**使用注解**：这个元素在 HTML4 和 XHTML1 中废除了，并且在 HTML5 中移除。如果语义上合适的话（也就是如果表示删除的内容），使用 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/del)来代替。在所有其它的情况中，使用[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/s)。



## 属性

这个元素仅仅包含 [全局属性](https://developer.mozilla.org/en-US/docs/HTML/Global_attributes)



## DOM 接口

这个元素实现了 [`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口。



**实现注解：**直到 Gecko 1.9.2（包含），Firefox 为这个元素实现了  [`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口。



## 示例

```javascript
<strike>Today's Special: Salmon</strike> SOLD OUT<br />
<s>Today's Special: Salmon</s> SOLD OUT
```

代码结果为：



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)1  | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)1              | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18