# hsetnx

```javascript
HSETNX key field value
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

设置`field`在哈希存储在`key`到`value`，只有当`field`不存在。如果`key`不存在，则创建一个保存散列的新密钥。如果`field`已经存在，则该操作不起作用。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1`如果`field`是散列中的新字段并且`value`已设置。

- `0`如果`field`已经存在于哈希中并且没有执行任何操作。

## 例子

redis> HSETNX myhash field "Hello" `(integer) 1` redis> HSETNX myhash field "World" `(integer) 0` redis> HGET myhash field `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18