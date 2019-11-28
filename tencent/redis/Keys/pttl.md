# pttl

```javascript
PTTL key
```

**自2.6.0起可用。**

**时间复杂度：** O（1）

与 TTL 类似，这个命令返回一个有效期的密钥的剩余时间，唯一不同的是 TTL 返回以毫秒为单位的剩余时间，以毫秒为单位。

在 Redis 2.6或更高版本中，`-1`如果密钥不存在或者密钥存在但没有关联到期，则该命令将返回。

从 Redis 2.8开始，错误发生时的返回值发生了变化：

- `-2`如果该键不存在，则该命令返回。

- `-1`如果密钥存在但没有关联过期，则该命令返回。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：TTL以毫秒为单位，或者为负值以指示错误（请参阅上述说明）。

## 例子

redis> SET mykey "Hello" `"OK"` redis> EXPIRE mykey 1 `(integer) 1` redis> PTTL mykey `(integer) 999`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  