# big

**已废弃**



此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

The HTML Big Element (`<big>`) 会使字体加大一号（例如从小号(small)到中号(medium)，从大号(large)到加大(x-large)），最大不超过浏览器的最大字体。



**使用备注:**由于它是纯显示性的，该元素在[HTML5](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)中已经被移除，不应当再使用。取而代之，网页开发者应当使用CSS属性。



## 属性

只包含 [全局属性](https://developer.mozilla.org/en-US/docs/HTML/global_attributes)



## 示例 1



```javascript
<p>
  This is the first sentence. <big>This whole 
  sentence is in bigger letters.</big>
</p>
```

## 示例 2 (CSS 版)



```javascript
<p>
  This is the first sentence. <span style="font-size:1.2em">This whole 
  sentence is in bigger letters.</span>
</p>
```

### 结果



这是第一句话。整个句子的字母更大。

## DOM 接口



该元素实现了[`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)接口.



**实现备注: 直到** Gecko 1.9.2, Firefox 为该元素实现了[`HTMLSpanElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSpanElement)接口.



## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes)  | (Yes) | (Yes)   | (Yes)             | (Yes) | (Yes)  |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes)   | (Yes)              | (Yes)       | (Yes)               | (Yes)     | (Yes)         | (Yes)      |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18