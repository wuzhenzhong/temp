# getrange

```javascript
GETRANGE key start end
```

**自2.4.0起可用。**

**时间复杂度：** O（N）其中N是返回字符串的长度。复杂度最终取决于返回的长度，但是因为从现有字符串创建子字符串非常便宜，所以对于小字符串它可以被认为是O（1）。

**警告**：此命令已重命名为 GETRANGE，它`SUBSTR`在 Redis 版本中调用`<= 2.0`。

返回存储在的字符串值的子字符串`key`，由偏移量`start`和`end`（都包含在内）确定。可以使用负偏移来提供从字符串末尾开始的偏移量。所以-1表示最后一个字符，-2表示倒数第二个字符等等。

[纠错](javascript:;)

该函数通过将结果范围限制为字符串的实际长度来处理超出范围的请求。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)

## 例子

redis> SET mykey "This is a string" `"OK"` redis> GETRANGE mykey 0 3 `"This"` redis> GETRANGE mykey -3 -1 `"ing"` redis> GETRANGE mykey 0 -1 `"This is a string"` redis> GETRANGE mykey 10 100 `"string"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18