# hvals

```javascript
HVALS key
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是散列的大小。

返回存储在中的哈希中的所有值`key`。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：散列值的列表，或`key`不存在的空列表。

## 例子

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HSET myhash field2 "World" `(integer) 1` redis> HVALS myhash `1) "Hello" 2) "World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18