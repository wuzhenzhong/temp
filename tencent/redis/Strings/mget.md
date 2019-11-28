# mget

```javascript
MGET key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是要检索的键的数量。

返回所有指定键的值。对于不包含字符串值或不存在的每个键，`nil`都会返回特殊值。因此，操作永远不会失败。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：指定键值的列表。

## 例子

redis> SET key1 "Hello" `"OK"` redis> SET key2 "World" `"OK"` redis> MGET key1 key2 nonexisting `1) "Hello" 2) "World" 3) (nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18