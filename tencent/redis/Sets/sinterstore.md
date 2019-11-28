# sinterstore

```javascript
SINTERSTORE destination key [key ...]
```

**自1.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N * M）最差情况，其中N是最小集合的基数，M是集合的数量。

该命令与 SINTER 相同，但不是返回结果集，而是存储在中`destination`。

如果`destination`已经存在，它将被覆盖。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：结果集中元素的数量。

## 例子

redis> SADD key1 "a" `(integer) 1` redis> SADD key1 "b" `(integer) 1` redis> SADD key1 "c" `(integer) 1` redis> SADD key2 "c" `(integer) 1` redis> SADD key2 "d" `(integer) 1` redis> SADD key2 "e" `(integer) 1` redis> SINTERSTORE key key1 key2 `(integer) 1` redis> SMEMBERS key `1) "c"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18