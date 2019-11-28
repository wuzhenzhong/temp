# flushdb

```javascript
FLUSHDB [ASYNC]
```

**自1.0.0起可用。**

删除当前所选数据库的所有键。该命令永远不会失败。

此操作的时间复杂度为 O（N），N 是数据库中的键数。

## `FLUSHDB ASYNC` （ Redis 4.0.0 或更高版本）

有关文档，请参阅 FLUSHALL 。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18