# frame

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



`<frame>` 是 HTML 元素，它定义了一个特定区域，另一个 HTML 文档可以在里面展示。帧应该在[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frameset)中使用。



`<frame> `的使用不应提倡，因为有一些缺点，比如性能问题，以及使用屏幕阅读器的用户缺少可访问性。比起`<frame>`，[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)更应该提倡。



## 属性

就像其他 HTML 元素一样，这个元素支持 [全局属性](https://developer.mozilla.org/en-US/docs/HTML/Global_attributes)。



**src** 这个属性指定了由帧展示的文档。

**name**这个属性用于给帧添加标签。如果没有标签，所有链接都会在它们所在的帧中打开。**noresize**这个属性避免让用户改变帧的大小。

**scrolling**这个属性定义了滚动条的存在。如果不使用这个属性，浏览器会按需放置滚动条。有两个选择：`"yes"` 用于展示滚动条，即使是多余的；`"no"` 用于不展示滚动条，即使它是必要的。

**marginheight**这个属性定义了帧之间的边距有多高。

**marginwidth**这个属性定义了帧之间的边距有多宽。

**frameborder**这个属性允许你为帧添加边框。



## 示例

```javascript
<frameset cols="50%,50%">
  <frame src="https://developer.mozilla.org/en/HTML/Element/iframe" />
  <frame src="https://developer.mozilla.org/en/HTML/Element/frame" />
</frameset>
```

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| frameborder   | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| marginheight  | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| marginwidth   | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| name          | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| noresize      | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| scrolling     | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| src           | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| frameborder   | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| marginheight  | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| marginwidth   | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| name          | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| noresize      | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| scrolling     | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| src           | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18