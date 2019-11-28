# command count

```javascript
COMMAND COUNT
```

**自2.8.13起可用。**

**时间复杂度：** O（1）

返回此 Redis 服务器中总数命令的[整数回复](https://redis.io/topics/protocol#integer-reply)。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：COMMAND 返回的命令数

## 例子

redis> COMMAND COUNT `(integer) 178`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18