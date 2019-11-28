# bitop

```javascript
BITOP operation destkey key [key ...]
```

**自2.6.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N）

在多个键（包含字符串值）之间执行按位操作并将结果存储在目标键中。

BITOP 命令支持四个按位运算：**AND**，**OR**，**XOR**和**NOT**，因此调用该命令的有效形式为：

- `BITOP AND destkey srckey1 srckey2 srckey3 ... srckeyN`

- `BITOP OR destkey srckey1 srckey2 srckey3 ... srckeyN`

- `BITOP XOR destkey srckey1 srckey2 srckey3 ... srckeyN`

- `BITOP NOT destkey srckey`

正如你可以看到，**NOT** 是特殊的，因为它只需要一个输入键，因为它执行比特反转，所以它只作为一元运算符有意义。

操作结果始终存储在`destkey`。

## 处理不同长度的字符串

当在具有不同长度的字符串之间执行操作时，比集合中最长的字符串短的所有字符串被视为零填充到最长字符串的长度。

对于不存在的密钥也是如此，这些密钥被视为零字节流直到最长字符串的长度。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)

存储在目标密钥中的字符串的大小，即等于最长输入字符串的大小。

## 例子

redis> SET key1 "foobar" `"OK"` redis> SET key2 "abcdef" `"OK"` redis> BITOP AND dest key1 key2 `(integer) 6` redis> GET dest `"`bc`ab"`

## 模式：使用位图的实时指标

BITOP 对 BITCOUNT 命令文档中记录的模式有很好的补充。可以组合不同的位图以获得执行总体计数操作的目标位图。

查看名为“ [使用Redis位图的快速简单实时指标](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps) ”的文章，了解一些有趣的用例。

## 性能考虑

BITOP 是一个可能很慢的命令，因为它在O（N）时间运行。对长输入字符串运行时应小心。

对于涉及大量输入的实时指标和统计信息，一种好的方法是使用一个从属（禁用只读选项）的位置执行位操作，以避免阻塞主实例。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18