# config resetstat

```javascript
CONFIG RESETSTAT
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

[纠错](javascript:;)

使用 INFO 命令重置由 Redis 报告的统计信息。

这些是重置的计数器：

- Keyspace hits

- Keyspace misses

- Number of commands processed

- Number of connections received

- Number of expired keys

- Number of rejected connections

- Latest fork(2) time

- The `aof_delayed_fsync` counter

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply): always `OK`.

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18