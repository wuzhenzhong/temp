# hlen

```javascript
HLEN key
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

返回存储在中的哈希中包含的字段数`key`。

[纠错](javascript:;)

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：散列中的字段数或不存在`0`时的字段数`key`。

## 例子

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HSET myhash field2 "World" `(integer) 1` redis> HLEN myhash `(integer) 2`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18