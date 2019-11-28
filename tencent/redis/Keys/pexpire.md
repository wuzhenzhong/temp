# pexpire

```javascript
PEXPIRE key milliseconds
```

**自2.6.0起可用。**

**时间复杂度：** O（1）

此命令与 EXPIRE 完全相同，但密钥的生存时间以毫秒而不是秒为单位指定。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果超时被设置。

- `0`如果`key`不存在。

## 例子

redis> SET mykey "Hello" `"OK"` redis> PEXPIRE mykey 1500 `(integer) 1` redis> TTL mykey `(integer) 1` redis> PTTL mykey `(integer) 1495`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  