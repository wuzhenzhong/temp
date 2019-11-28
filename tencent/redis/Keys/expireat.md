# expireat

```javascript
EXPIREAT key timestamp
```

**自1.2.0起可用。**

**时间复杂度：** O（1）

EXPIREA T与 EXPIRE 具有相同的效果和语义，但不是指定表示TTL（生存时间）的秒数，而是需要绝对的 [Unix时间戳](http://en.wikipedia.org/wiki/Unix_time)（1970年1月1日以来的秒数）。过去的时间戳会立即删除密钥。

有关该命令的具体语义，请参阅 EXPIRE 的文档。

## 背景

引入 EXPIREAT 是为了将相对超时转换为 AOF 持久模式的绝对超时。当然，它可以直接用来指定给定密钥将在特定时间到期。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果超时被设置。

- `0`如果`key`不存在。

## 例子

redis> SET mykey "Hello" `"OK"` redis> EXISTS mykey `(integer) 1` redis> EXPIREAT mykey 1293840000 `(integer) 1` redis> EXISTS mykey `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  