# type

```javascript
TYPE key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

返回存储在的值的类型的字符串表示形式`key`。可以返回不同的类型有：`string`，`list`，`set`，`zset`和`hash`。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：类型`key`或`none`时`key`不存在。

## 例子

redis> SET key1 "value" `"OK"` redis> LPUSH key2 "value" `(integer) 1` redis> SADD key3 "value" `(integer) 1` redis> TYPE key1 `"string"` redis> TYPE key2 `"list"` redis> TYPE key3 `"set"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  