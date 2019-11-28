# psubscribe

```javascript
PSUBSCRIBE pattern [pattern ...]
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中N是客户端已订阅的模式数量。

订阅客户端给定的模式。

支持的全局样式模式：

- `h?llo`订阅到`hello`，`hallo`和`hxllo`

- `h*llo`订阅到`hllo`和`heeeello`

- `h[ae]llo`订阅到`hello`，`hallo,`但不是`hillo`

如果您想逐字匹配，请使用`\`转义特殊字符。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18