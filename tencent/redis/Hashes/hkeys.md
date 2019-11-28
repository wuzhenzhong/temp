# hkeys

```javascript
HKEYS key
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是散列的大小。

返回存储在中的哈希中的所有字段名称`key`。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：散列中的字段列表，或空列表`key`不存在时。

## 例子

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HSET myhash field2 "World" `(integer) 1` redis> HKEYS myhash `1) "field1" 2) "field2"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18