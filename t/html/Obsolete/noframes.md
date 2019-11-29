# noframes

`<noframes>` 是个 HTML 元素，用于支持不支持  [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frame)元素的浏览器，或者这样配置的浏览器。



你可以在`<noframes>` 中使用任何 HTML 元素，它预期可以在[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body)中看到，除了[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frameset)和[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frame)元素。



**注：**由于所有主流浏览器都支持帧，这个元素一般不需要使用。它也在 HTML5 中完全过时，并且应该避免使用，来遵循标准。



## 属性

就像其它 HTML 元素那样，这个元素支持 [全局属性](https://developer.mozilla.org/en-US/HTML/Global_attributes)。



## 示例

```javascript
<frameset cols="50%,50%">
  <frame src="https://developer.mozilla.org/en/HTML/Element/frameset" />
  <frame src="https://developer.mozilla.org/en/HTML/Element/frame" />
  <noframes><p>It seems your browser does not support frames or is not configured do so.</p></noframes>
</frameset>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18