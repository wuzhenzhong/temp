# spop

```javascript
SPOP key [count]
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

[纠错](javascript:;)

从设置值存储中移除并返回一个或多个随机元素`key`。

该操作与 SRANDMEMBER 相似，它返回一个或多个随机元素，但不会将其删除。

该`count`参数自 3.2 版开始可用。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：已移除的元素或`nil`何时`key`不存在。

## 例子

redis> SADD myset "one" `(integer) 1` redis> SADD myset "two" `(integer) 1` redis> SADD myset "three" `(integer) 1` redis> SPOP myset `"one"` redis> SMEMBERS myset `1) "two" 2) "three"` redis> SADD myset "four" `(integer) 1` redis> SADD myset "five" `(integer) 1` redis> SPOP myset 3 `1) "four" 2) "three" 3) "five"` redis> SMEMBERS myset `1) "two"`

## 计数通过时的行为规范

如果 count 大于 Set 内部的元素数量，则该命令将只返回整个集合而不包含其他元素。

## 返回元素的分配

请注意，当您需要保证返回元素的均匀分布时，此命令不适用。有关用于 SPOP 的算法的更多信息，请查找 Knuth 采样和 Floyd 采样算法。

## 计数参数扩展

Redis 3.2引入了一个可选`count`参数，可以传递给 SPOP 以便在一次调用中检索多个元素。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18