# sismember

```javascript
SISMEMBER key member
```

**自1.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

如果`member`是存储在中的集合的成员，则返回`key`。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果元素是集合的成员。

- `0`如果元素不是集合的成员，或者`key`不存在。

## 例子

redis> SADD myset "one" `(integer) 1` redis> SISMEMBER myset "one" `(integer) 1` redis> SISMEMBER myset "two" `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18