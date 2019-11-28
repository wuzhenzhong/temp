# hgetall

```javascript
HGETALL key
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是散列的大小。

返回存储在中的哈希的所有字段和值`key`。在返回的值中，每个字段名称的后面跟着它的值，所以回复的长度是散列大小的两倍。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：存储在散列中的字段及其值的列表，或`key`不存在时的空列表。

## 例子

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HSET myhash field2 "World" `(integer) 1` redis> HGETALL myhash `1) "field1" 2) "Hello" 3) "field2" 4) "World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18