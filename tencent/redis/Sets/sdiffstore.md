# sdiffstore

```javascript
SDIFFSTORE destination key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是所有给定集合中元素的总数。

此命令等于 SDIFF，但不是返回结果集，而是存储在中`destination`。

如果`destination`已经存在，它将被覆盖。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：结果集中元素的数量。

## 例子

redis> SADD key1 "a" `(integer) 1` redis> SADD key1 "b" `(integer) 1` redis> SADD key1 "c" `(integer) 1` redis> SADD key2 "c" `(integer) 1` redis> SADD key2 "d" `(integer) 1` redis> SADD key2 "e" `(integer) 1` redis> SDIFFSTORE key key1 key2 `(integer) 2` redis> SMEMBERS key `1) "a" 2) "b"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18