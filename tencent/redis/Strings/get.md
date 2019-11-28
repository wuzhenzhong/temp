# get

```javascript
GET key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

获得的价值`key`。如果该键不存在，`nil`则返回特殊值。如果存储的值`key`不是字符串，则会返回错误，因为 GET 仅处理字符串值。

## 返回值

[散装串答复](https://redis.io/topics/protocol#bulk-string-reply)：值`key`，或者`nil`当`key`不存在。

## 例子

redis> GET nonexisting `(nil)` redis> SET mykey "Hello" `"OK"` redis> GET mykey `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18