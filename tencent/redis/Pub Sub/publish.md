# publish

```javascript
PUBLISH channel message
```

**自2.0.0起可用。**

**时间复杂度：** O（N + M）其中N是订阅接收频道的客户数量，M是订购模式的总数（由任何客户）。

向给定频道发布消息。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：收到消息的客户端数量。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18