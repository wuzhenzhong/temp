# sadd

```javascript
SADD key member [member ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（1）为每个元素添加，所以O（N）在用多个参数调用该命令时添加N个元素。

将指定的成员添加到存储在的集合中`key`。指定的已经是该组成员的成员将被忽略。如果`key`不存在，则在添加指定成员之前创建一个新集。

当存储的值`key`不是集合时，将返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：添加到集合中的元素数量，不包括集合中已经存在的所有元素。

## 历史

- `>= 2.4`：接受多个`member`参数。2.4之前的 Redis 版本只能为每个呼叫添加一个成员。

## 例子

redis> SADD myset "Hello" `(integer) 1` redis> SADD myset "World" `(integer) 1` redis> SADD myset "World" `(integer) 0` redis> SMEMBERS myset `1) "Hello" 2) "World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18