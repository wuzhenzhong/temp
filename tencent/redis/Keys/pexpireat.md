# pexpireat

```javascript
PEXPIREAT key milliseconds-timestamp
```

**自2.6.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

PEXPIREAT与EXPIREAT具有相同的效果和语义，但密钥过期的Unix时间以毫秒而非秒为单位指定。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果超时被设置。

- `0`如果`key`不存在。

## 例子

redis> SET mykey "Hello" `"OK"` redis> PEXPIREAT mykey 1555555555005 `(integer) 1` redis> TTL mykey `(integer) 49282964` redis> PTTL mykey `(integer) 49282964157`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  