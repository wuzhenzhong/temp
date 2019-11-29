# frameset

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



[纠错](javascript:;)

`<frameset>`是一个用于包含[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frame)的 HTML 元素。



**注意:** 现在不鼓励使用 frame，而是用[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)。现今的网站一般都不使用 frame。



## 属性

像所有其他的HTML元素一样，这个元素支持[全局属性](https://developer.mozilla.org/en-US/HTML/Global_attributes)。



**cols**这个属性指定一个框架集中列的数目和尺寸。

**rows**这个属性指定一个框架集中行的数目和尺寸。



## 示例

```javascript
<frameset cols="50%,50%">
  <frame src="https://developer.mozilla.org/en/HTML/Element/frameset" />
  <frame src="https://developer.mozilla.org/en/HTML/Element/frame" />
</frameset>
```

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| cols          | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |
| rows          | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| cols          | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |
| rows          | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18