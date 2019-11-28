# hincrby

```javascript
HINCRBY key field increment
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

递增`field`存储在`key`by 处的散列中的数字`increment`。如果`key`不存在，则创建一个保存散列的新密钥。如果`field`不存在，则`0`在执行操作之前将该值设置为。

HINCRBY 支持的值范围限于64位有符号整数。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：`field`递增操作后的值。

## 例子

自`increment`变量被签名后，可以执行递增和递减操作：

redis> HSET myhash field 5 `(integer) 1` redis> HINCRBY myhash field 1 `(integer) 6` redis> HINCRBY myhash field -1 `(integer) 5` redis> HINCRBY myhash field -10 `(integer) -5`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18