# cluster reset（集群重置）

```javascript
CLUSTER RESET [HARD|SOFT]
```

**自3.0.0起可用。**

**时间复杂度：** O（N）其中N是已知节点的数量。该命令可以执行 FLUSHALL 作为副作用。

重置 Redis 群集节点，根据复位类型，或多或少有些激烈，可能**很难**或很**软**。请注意，**如果主设备拥有一个或多个密钥**，**则**此命令**不起作用**，在这种情况下，要完全重置主节点密钥，必须首先移除密钥，例如先使用 FLUSHALL，然后使用 CLUSTER RESET。

对节点的影响：

\1. 群集中的所有其他节点都将被遗忘。

\2. 所有分配的/开放的时隙都被重置，因此插槽到节点的映射被完全清除。

\3. 如果节点是从属节点，则它变成一个（空）主节点。它的数据集被刷新，所以最后节点将是一个空主。

\4. **仅硬复位**：生成新的节点ID。

\5. **硬复位只**：`currentEpoch`和`configEpoch`增值经销商都设置为0。

\6. 新配置在节点群集配置文件的磁盘上保存。

此命令主要用于重新配置 Redis 群集节点，以便在新的不同群集的上下文中使用。每次执行新测试单元时，Redis Cluster 测试框架还广泛使用该命令以重置集群的状态。

如果未指定重置类型，则默认为**软**。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果命令成功。否则会返回错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18