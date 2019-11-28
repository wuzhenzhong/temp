# cluster getkeysinslot

```javascript
CLUSTER GETKEYSINSLOT slot count
```

**自3.0.0起可用。**

**时间复杂度：** O（log（N））其中 N 是请求的键的数量

该命令返回存储在联系节点中的密钥名称数组，并哈希到指定的哈希槽。通过`count`参数指定要返回的最大键数，以便 该 API 的用户可以批量处理键。

此命令的主要用途是在将集群插槽从一个节点重新组合到另一个节点期间。重做哈希的方式在 Redis 集群规范中公开，或者以更简单的摘要形式公开，作为CLUSTER SETSLOT 命令文档的附录。

```javascript
> CLUSTER GETKEYSINSLOT 7000 3
"47344|273766|70329104160040|key_39015"
"47344|273766|70329104160040|key_89793"
"47344|273766|70329104160040|key_92937"
```

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：从0到 Redis 数组回复中的密钥名称*计数*。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18