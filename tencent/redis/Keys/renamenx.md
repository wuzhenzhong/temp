# renamenx

```javascript
RENAMENX key newkey
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

重命名`key`为`newkey`如果`newkey`还不存在。它`key`不存在时会返回错误。

**注意：**在Redis 3.2.0之前，如果源和目标名称相同，则会返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1`如果`key`被重命名为`newkey`。

- `0`如果`newkey`已经存在。

## 例子

redis> SET mykey "Hello" `"OK"` redis> SET myotherkey "World" `"OK"` redis> RENAMENX mykey myotherkey `(integer) 0` redis> GET myotherkey `"World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  