# cluster countkeysinslot

```javascript
CLUSTER COUNTKEYSINSLOT slot
```

**自3.0.0起可用。**

**时间复杂度：** O（1）

返回指定的 Redis Cluster 哈希槽中的键的数量。该命令仅查询本地数据集，因此联系未提供指定散列槽的节点将始终导致返回计数为零。

```javascript
> CLUSTER COUNTKEYSINSLOT 7000
(integer) 50341
```

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：指定哈希槽中的密钥数量，如果哈希槽无效，则返回错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18