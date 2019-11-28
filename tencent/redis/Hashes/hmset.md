# hmset

```javascript
HMSET key field value [field value ...]
```

**自2.0.0起可用。**

**时间复杂度：** O（N）其中 N 是设置的字段数。

将指定的字段设置为存储在其中的哈希中各自的值`key`。该命令覆盖散列中已存在的任何指定字段。如果`key`不存在，则创建一个保存散列的新密钥。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> HMSET myhash field1 "Hello" field2 "World" `"OK"` redis> HGET myhash field1 `"Hello"` redis> HGET myhash field2 `"World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18