# select

```javascript
SELECT index
```

**自1.0.0起可用。**

选择具有指定的基于零的数字索引的 Redis 逻辑数据库。新连接始终使用数据库0。

Redis 不同的可选数据库是命名空间的一种形式：无论如何，所有的数据库都保存在同一个 RDB / AOF 文件中。但是，不同的数据库可以具有相同名称的密钥，并且可以使用 FLUSHDB ， SWAPDB 或 RANDOMKEY 等可用于特定数据库的命令。

实际上，如果需要，Redis 数据库应主要用于分隔属于同一应用程序的不同密钥，而不是为多个不相关的应用程序使用单个 Redis 实例。

使用 Redis 集群时，不能使用 SELECT 命令，因为 Redis 集群仅支持数据库零。就 Redis 集群而言，拥有多个数据库将毫无用处，而且是一个无价值的复杂资源，因为无论如何，在 Redis 集群的设计和目标下，无法在单个数据库上自动运行命令。

由于当前选择的数据库是连接的属性，因此客户端应跟踪当前选定的数据库并在重新连接时重新选择它。虽然没有命令来查询当前连接中选定的数据库，但 CLIENT LIST 输出显示当前选定的数据库的每个客户端。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18