# decr

```javascript
DECR key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

将存储的数字减`key`1。如果密钥不存在，则`0`在执行操作之前将其设置为。如果密钥包含错误类型的值或包含无法表示为整数的字符串，则会返回错误。该操作仅限于**64位有符号整数**。

[纠错](javascript:;)

有关增量/减量操作的额外信息，请参阅 INCR。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：`key`减量后的值

## 例子

redis> SET mykey "10" `"OK"` redis> DECR mykey `(integer) 9` redis> SET mykey "234293482390480948029348230948" `"OK"` redis> DECR mykey `ERR ERR value is not an integer or out of range`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18