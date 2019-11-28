# hset

```javascript
HSET key field value
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

设置`field`在存储在哈希`key`来`value`。如果`key`不存在，则创建一个保存散列的新密钥。如果`field`已经存在于散列中，它将被覆盖。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1`如果`field`是散列中的新字段并且`value`已设置。

- `0`如果`field`已经存在于散列中并且值已更新。

## 例子

redis> HSET myhash field1 "Hello" `(integer) 1` redis> HGET myhash field1 `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18