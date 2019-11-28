# rename

```javascript
RENAME key newkey
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

重命名`key`为`newkey`。它`key`不存在时会返回错误。如果`newkey`已经存在，它将被覆盖，当发生这种情况时，RENAME 执行一个隐式的 DEL 操作，所以如果被删除的键包含一个非常大的值，那么即使 RENAME 本身通常是一个常量操作，也可能导致高延迟。

**注意：**在 Redis 3.2.0之前，如果源和目标名称相同，则会返回错误。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> SET mykey "Hello" `"OK"` redis> RENAME mykey myotherkey `"OK"` redis> GET myotherkey `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  