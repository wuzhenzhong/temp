# blink

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



**已废弃**



此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

HTML Blink Element (`<blink>`)不是标准元素，它会使包含其中的文本闪烁。



不要使用这个元素，它已经被**淘汰**。闪烁字体不符合无障碍标准，CSS规范允许浏览器忽略闪烁值。



## DOM 接口



该元素不被支持，因而实现了[`HTMLUnknownElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLUnknownElement)接口.



## 示例

```javascript
<blink>Why would somebody use this?</blink>
```

### 结果 (淡化!)



![img](https://ask.qcloudimg.com/http-save/devdocs/8s53ks4tea.gif)

## 规范

该元素不是标准元素，不属于规范的一部分. 不信的话，[ 你自己来看HTML规范文档](http://www.whatwg.org/specs/web-apps/current-work/multipage/obsolete.html#non-conforming-features).



## CSS Polyfill

如果你真的需要一个polyfill，那么你可以使用下面的CSSPolyfill。适用于IE10 +。

```javascript
blink {
    -webkit-animation: 2s linear infinite condemned_blink_effect; // for android
    animation: 2s linear infinite condemned_blink_effect;
}
@-webkit-keyframes condemned_blink_effect { // for android
    0% {
        visibility: hidden;
    }
    50% {
        visibility: hidden;
    }
    100% {
        visibility: visible;
    }
}
@keyframes condemned_blink_effect {
    0% {
        visibility: hidden;
    }
    50% {
        visibility: hidden;
    }
    100% {
        visibility: visible;
    }
}
```

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera  | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :----- | :----- |
| Basic Support | No     | No   | 1 — 22  | No                | 1 — 15 | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | 1 — 22              | No        | 1 — 15        | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18