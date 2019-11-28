# ttl

```javascript
TTL key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

返回具有超时的密钥的剩余生存时间。这种内省功能允许Redis客户端检查给定密钥将继续成为数据集的一部分的秒数。

在 Redis 2.6 或更高版本中，`-1`如果密钥不存在或者密钥存在但没有关联到期，则该命令将返回。

从 Redis 2.8 开始，错误发生时的返回值发生了变化：

- `-2`如果该键不存在，则该命令返回。

- `-1`如果密钥存在但没有关联过期，则该命令返回。

另请参阅以毫秒分辨率返回相同信息的 PTTL 命令（仅在 Redis 2.6 或更高版本中可用）。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：TTL 以秒为单位，或者为负值以指示错误（请参阅上述说明）。

## 例子

redis> SET mykey "Hello" `"OK"` redis> EXPIRE mykey 10 `(integer) 1` redis> TTL mykey `(integer) 10`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  