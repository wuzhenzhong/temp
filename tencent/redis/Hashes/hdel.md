# hdel

```javascript
HDEL key field [field ...]
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是要删除的字段数。

从中存储的哈希中删除指定的字段`key`。此哈希中不存在的指定字段将被忽略。如果`key`不存在，则将其视为空散列，并返回此命令`0`。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：从散列中删除的字段数量，不包括指定但不存在的字段。

## History

- `>= 2.4`：接受多个`field`参数。早于2.4版的 Redis 版本只能删除每个呼叫的字段。

要在早期版本中以原子方式从哈希中删除多个字段，请使用 MULTI / EXEC 块。

## 例子

redis> HSET myhash field1 "foo" `(integer) 1` redis> HDEL myhash field1 `(integer) 1` redis> HDEL myhash field2 `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18