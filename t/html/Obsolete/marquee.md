# marquee

**已废弃**

此功能已过时。虽然它可能在某些浏览器中仍然有效，但由于可以在任何时候删除它，所以不鼓励使用它。尽量避免使用。

HTML marquee 元素（`<marquee>）`用来插入一段滚动的文字。你可以使用它的属性控制当文本到达容器边缘发生的事情。



`<marquee>`元素已经 **过时**，请不要再使用。尽管一些浏览器仍然支持它，但它不是必须的。

## 属性

**behavior**设置文本在 marquee 元素内如何滚动。可选值有 `scroll，slide`和 `alternate。` 如果未指定值，默认值为 `scroll。`

**bgcolor**通过颜色名称或十六进制值设置背景颜色。

**direction**设置 marquee 内文本滚动的方向。可选值有 `left`,`right`,`up`and`down。`如果未指定值，默认值为 `left。`

**height**以像素或百分比值设置高度。

**hspace**设置水平边距。

**loop**设置 marquee 滚动的次数。如果未指定值，默认值为 −1，表示 marquee 将连续滚动.

**scrollamount**设置每次滚动时移动的长度（以像素为单位）。默认值为 6。

**scrolldelay**设置每次滚动时的时间间隔（以毫秒为单位）。默认值为 85。请注意， 除非指定 truespeed 值，否则将忽略任何小于 60 的值，并改为使用 60。

**truespeed**默认情况下，会忽略小于60的scrolldelay值。如果存在truespeed，那些值不会被忽略。

**vspace**以像素或百分比值设置垂直边距。

**width**以像素或百分比值设置宽度。



## 事件回调



**onbounce**当 marquee 滚动到结尾时触发。它只能在 behavior 属性设置为 alternate 时触发。

**onfinish**当 marquee 完成 loop 属性设置的值时触发。它只能在 loop 属性设置为大于 0 的某个数字时触发。

**onstart**当 marquee 开始滚动时触发。



## 方法

start开始滚动 marquee。stop停止滚动 marquee。



## 示例

```javascript
<marquee>This text will scroll from right to left</marquee>

<marquee direction="up">This text will scroll from bottom to top</marquee>

<marquee direction="down" width="250" height="200" behavior="alternate" style="border:solid">
  <marquee behavior="alternate">
    This text will bounce
  </marquee>
</marquee>
```

## 规范

| 规范                                            | 状态     | 评论                                                        |
| :---------------------------------------------- | :------- | :---------------------------------------------------------- |
| HTML Living Standard该规范中'<marquee>'的定义。 | 生活水平 | 为了向后兼容，使其过时而倾向于CSS，但是定义了它的预期行为。 |
| HTML5该规范中'<marquee>'的定义。                | 建议     | 为了向后兼容，使其过时而倾向于CSS，但是定义了它的预期行为。 |

## 浏览器兼容性

| Feature       | Chrome | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| behavior      | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| bgcolor       | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| direction     | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| height        | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| hspace        | ?      | (Yes) | 3       | ?                 | ?     | ?      |
| loop          | ?      | (Yes) | 3       | ?                 | ?     | ?      |
| scrollamount  | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| scrolldelay   | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |
| truespeed     | No     | (Yes) | 3       | 4                 | No    | No     |
| vspace        | ?      | (Yes) | 3       | ?                 | ?     | ?      |
| width         | 1      | (Yes) | 1       | 2                 | 7.2   | 1.2    |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| behavior      | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| bgcolor       | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| direction     | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| height        | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| hspace        | ?       | ?                  | (Yes)       | 3                   | ?         | ?             | ?          |
| loop          | ?       | ?                  | (Yes)       | 3                   | ?         | ?             | ?          |
| scrollamount  | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| scrolldelay   | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |
| truespeed     | ?       | ?                  | (Yes)       | 3                   | ?         | ?             | ?          |
| vspace        | ?       | ?                  | (Yes)       | 3                   | ?         | ?             | ?          |
| width         | ?       | ?                  | (Yes)       | 1                   | ?         | ?             | ?          |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18