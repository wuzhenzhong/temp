# sunionstore

```javascript
SUNIONSTORE destination key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是所有给定集合中元素的总数。

该命令与SUNION相同，但不是返回结果集，而是存储在中`destination`。

如果`destination`已经存在，它将被覆盖。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：结果集中元素的数量。

## 例子

redis> SADD key1 "a" `(integer) 1` redis> SADD key1 "b" `(integer) 1` redis> SADD key1 "c" `(integer) 1` redis> SADD key2 "c" `(integer) 1` redis> SADD key2 "d" `(integer) 1` redis> SADD key2 "e" `(integer) 1` redis> SUNIONSTORE key key1 key2 `(integer) 5` redis> SMEMBERS key `1) "a" 2) "c" 3) "e" 4) "b" 5) "d"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18