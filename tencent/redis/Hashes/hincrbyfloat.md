# hincrbyfloat

```javascript
HINCRBYFLOAT key field increment
```

**自2.6.0起可用。**

**时间复杂度：** O（1）

按指定增加`field`存储在指定位置的哈希`key`，并表示浮点数`increment`。如果增量值为负值，则结果是使散列字段值**递减**而不递增。如果该字段不存在，则`0`在执行操作之前将其设置为。如果发生以下情况之一，则会返回错误：

- 该字段包含错误类型的值（不是字符串）。

- 当前字段内容或指定的增量不可解析为双精度浮点数。

此命令的确切行为与 INCRBYFLOAT 命令的完全相同，请参阅 INCRBYFLOAT 文档以获取更多信息。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：`field`增量后的值。

## 例子

redis> HSET mykey field 10.50 `(integer) 1` redis> HINCRBYFLOAT mykey field 0.1 `"10.6"` redis> HINCRBYFLOAT mykey field -5 `"5.6"` redis> HSET mykey field 5.0e3 `(integer) 0` redis> HINCRBYFLOAT mykey field 2.0e2 `"5200"`

## 实施细节

该命令始终在复制链接和仅附加文件中作为 HSET 操作进行传播，因此基础浮点数学实现中的差异不会成为不一致性的来源。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18