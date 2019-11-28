# hmget

```javascript
HMGET key field [field ...]
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是请求的字段数。

返回与在`fields`中存储的哈希中指定的值`key`。

对于`field`散列中不存在的每一个，`nil`都会返回一个值。因为不存在的键被视为空散列，所以对不存在的键运行 HMGET `key`会返回`nil`值列表。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：与给定字段关联的值列表，顺序与请求的顺序相同。

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HSET myhash field2 "World" `(integer) 1` redis> HMGET myhash field1 field2 nofield `1) "Hello" 2) "World" 3) (nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18