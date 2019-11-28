# dump

```javascript
DUMP key
```

**自2.6.0起可用。**

**时间复杂度：** O（1）访问密钥和附加的O（N * M）来序列化它，其中N是组成该值的Redis对象的数量，M是它们的平均大小。对于小字符串值，时间复杂度因此是O（1）+ O（1 * M），其中M很小，因此简单地为O（1）。

使用 Redi s特定格式序列化存储在密钥中的值并将其返回给用户。可以使用 RESTORE 命令将返回的值合成回 Redis 密钥。

序列化格式是不透明和非标准的，但它具有一些语义特征：

- 它包含一个64位校验和，用于确保检测到错误。RESTORE 命令确保在使用序列化值合成密钥之前检查校验和。

- 值的编码格式与RDB使用的格式相同。

- RDB版本在序列化的值内编码，因此具有不兼容的RDB格式的不同Redis版本将拒绝处理序列化的值。

序列化的值不包含过期信息。为了捕捉当前值的生存时间，应该使用PTTL命令。

如果`key`不存在，则返回零批量答复。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：序列化的值。

## 例子

redis> SET mykey 10 `"OK"` redis> DUMP mykey `"\u0000\xC0\n\b\u0000ײ\xBB\xFA\xA7\xB7\xE9\x83"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18