# hexists

```javascript
HEXISTS key field
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

返回是否`field`存储在哈希中的现有字段`key`。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1`如果散列包含`field`。

- `0`如果散列不包含`field`或`key`不存在。

## 例子

redis> HSET myhash field1 "foo" `(integer) 1` redis> HEXISTS myhash field1 `(integer) 1` redis> HEXISTS myhash field2 `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18