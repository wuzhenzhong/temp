# bgrewriteaof

```javascript
BGREWRITEAOF
```

**自1.0.0起可用。**

指示 Redis 启动[仅附加文件](https://redis.io/topics/persistence#append-only-file)重写过程。重写将创建当前仅附加文件的小型优化版本。

如果 BGREWRITEAOF 失败，则不会丢失任何数据，因为旧的 AOF 将保持不变。

如果还没有后台进程正在进行持久化，则重写将仅由 Redis 触发。特别：

- 如果 Redis 子*进程*在磁盘上创建快照，则 AOF 重写*将按计划进行，*但在生成 RDB 文件的保存子终止之前不会启动。在这种情况下，BGREWRITEAOF 仍然会返回一个 OK 代码，但是会显示相应的消息。从 Redis 2.6 开始，您可以检查是否计划了 AOF 重写，查看 INFO 命令。

- 如果 AOF 重写正在进行中，该命令会返回一个错误，并且以后不会安排 AOF 重写。

自 Redis 2.4 起，AOF 重写由 Redis 自动触发，但 BGREWRITEAOF 命令可随时用于触发重写。

有关详细信息，请参阅[持久性文档](https://redis.io/topics/persistence)。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply): always `OK`.

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18