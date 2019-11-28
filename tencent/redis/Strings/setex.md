# setex

```javascript
SETEX key seconds value
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

设置`key`为保留字符串`value`并`key`在给定秒数后设置为超时。该命令相当于执行以下命令：

```javascript
SET mykey value
EXPIRE mykey seconds
```

SETEX 是原子的，可以通过使用 MULTI / EXEC 块内的前两个命令来重现。它作为给定操作序列的一个更快的替代方式提供，因为当 Redis 用作缓存时，此操作非常常见。

`seconds`无效时返回错误。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> SETEX mykey 10 "Hello" `"OK"` redis> TTL mykey `(integer) 10` redis> GET mykey `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18