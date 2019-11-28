# strlen

```javascript
STRLEN key
```

**自2.2.0起可用。**

**时间复杂度：** O（1）

返回存储在的字符串值的长度`key`。`key`保存非字符串值时会返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：字符串的长度`key`，或者`0`当`key`不存在时。

## 例子

redis> SET mykey "Hello world" `"OK"` redis> STRLEN mykey `(integer) 11` redis> STRLEN nonexisting `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18