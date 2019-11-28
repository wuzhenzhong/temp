# cluster keyslot（集群键槽）

```javascript
CLUSTER KEYSLOT key
```

**自3.0.0起可用。**

**时间复杂度：** O（N）其中N是密钥中的字节数

返回一个整数，用于标识指定密钥散列到的散列槽。该命令主要用于调试和测试，因为它通过 API 公开了哈希算法的底层 Redis 实现。此命令的示例用例：

\1. 客户端库可以使用 Redis 来测试他们自己的散列算法，生成随机密钥并用它们的本地实现和使用 Redis CLUSTER KEYSLOT 命令散列它们，然后检查结果是否相同。

\2. 人类可以使用此命令来检查什么是散列槽，然后是关联的 Redis 集群节点，负责给定的密钥。

## 示例

```javascript
> CLUSTER KEYSLOT somekey
11058
> CLUSTER KEYSLOT foo{hash_tag}
(integer) 2515
> CLUSTER KEYSLOT bar{hash_tag}
(integer) 2515
```

请注意，该命令实现了完整的散列算法，包括对**散列标签的**支持，这是 Redis Cluster 密钥散列算法的特殊属性，用于哈希中间的内容`{`以及`}`如果在密钥名称内找到这样的模式，以便强制多个键由同一个节点处理。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：散列槽号。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18