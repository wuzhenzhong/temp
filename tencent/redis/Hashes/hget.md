# hget

```javascript
HGET key field
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

返回`field`存储在哈希上的值`key`。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：与哈希关联的值`field`，或`nil`当`field`哈希`key`中不存在或不存在时。

## 例子

redis> HSET myhash field1 "foo" `(integer) 1` redis> HGET myhash field1 `"foo"` redis> HGET myhash field2 `(nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18