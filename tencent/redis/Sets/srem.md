# srem

```javascript
SREM key member [member ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是要移除的成员的数量。

从存储的集合中删除指定的成员`key`。指定的成员不是该组的成员将被忽略。如果`key`不存在，则将其视为空集，并返回此命令`0`。

当存储的值`key`不是集合时，将返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：从集合中删除的成员数量，不包括不存在的成员。

## 历史

- `>= 2.4`：接受多个`member`参数。早于2.4版的 Redis 版本只能删除每个呼叫的集合成员。

## 例子

redis> SADD myset "one" `(integer) 1` redis> SADD myset "two" `(integer) 1` redis> SADD myset "three" `(integer) 1` redis> SREM myset "one" `(integer) 1` redis> SREM myset "four" `(integer) 0` redis> SMEMBERS myset `1) "two" 2) "three"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18