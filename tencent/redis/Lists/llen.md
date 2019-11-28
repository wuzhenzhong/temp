# llen

```javascript
LLEN key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

返回存储在列表中的长度`key`。如果`key`不存在，则将其解释为空列表并`0`返回。当存储的值`key`不是列表时，将返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：列表的长度`key`。

## 例子

redis> LPUSH mylist "World" `(integer) 1` redis> LPUSH mylist "Hello" `(integer) 2` redis> LLEN mylist `(integer) 2`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18