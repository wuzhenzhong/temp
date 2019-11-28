# save

```javascript
SAVE
```

**自1.0.0起可用。**

SAVE 命令执行数据集的**同步**保存，以 RDB 文件的形式生成 Redis 实例内部所有数据的*时间点*快照。

您几乎从不想在生产环境中调用SAVE来阻止所有其他客户端。通常使用 BGSAVE 。但是，如果遇到阻止 Redis 创建后台保存子进程的问题（例如 fork（2）系统调用中的错误），SAVE 命令可能是执行最新数据集转储的最后手段。

有关详细信息，请参阅[持久性文档](https://redis.io/topics/persistence)。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply): The commands returns OK on success.

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18