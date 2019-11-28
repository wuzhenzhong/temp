# subscribe

```javascript
SUBSCRIBE channel [channel ...]
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中N是要订阅的频道数量。

订阅客户端到指定的频道。

一旦客户端进入订阅状态，它不应该发出任何其他命令，除了额外的 SUBSCRIBE，PSUBSCRIBE，UNSUBSCRIBE 和 PUNSUBSCRIBE 命令。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18