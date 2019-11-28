# flushall

```javascript
FLUSHALL [ASYNC]
```

**自1.0.0起可用。**

[纠错](javascript:;)

删除所有现有数据库的所有密钥，而不仅仅是当前选择的数据库。该命令永远不会失败。

此操作的时间复杂度为 O（N），N 是所有现有数据库中的键数。

## `FLUSHALL ASYNC` （ Redis 4.0.0 或更高版本）

Redis 现在能够在不阻塞服务器的情况下删除不同线程后台的密钥。`ASYNC` FLUSHALL 和 FLUSHDB 添加了一个选项，以便让整个数据集或单个数据库异步释放。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18