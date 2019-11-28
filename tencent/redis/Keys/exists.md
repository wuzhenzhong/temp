# exists

```javascript
EXISTS key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

返回如果`key`存在。

自 Redis 3.0.3 以来，可以指定多个密钥而不是单个密钥。在这种情况下，它会返回现有密钥的总数。请注意，为单个键返回1或0只是可变参数使用的特例，因此该命令完全向后兼容。

用户应该知道，如果多次在参数中提及相同的现有密钥，它将被多次计数。所以如果`somekey`存在，`EXISTS somekey somekey`将返回2。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果密钥存在。

- `0` 如果密钥不存在。

自 Redis 3.0.3 以来，该命令接受可变数量的键并将返回值进行了概括：

- 存在于指定为参数的键中的键的数量。键提及多次，现有计数多次。

## 例子

redis> SET key1 "Hello" `"OK"` redis> EXISTS key1 `(integer) 1` redis> EXISTS nosuchkey `(integer) 0` redis> SET key2 "World" `"OK"` redis> EXISTS key1 key2 nosuchkey `(integer) 2`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18