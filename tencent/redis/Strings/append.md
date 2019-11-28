# append

```javascript
APPEND key value
```

**自2.0.0起可用。**

**时间复杂度：** O（1）。分摊的时间复杂度为O（1），假设附加值很小，并且已有值为任意大小，因为Redis使用的动态字符串库将使每次重新分配的可用空间加倍。

如果`key`已经存在，并且是一个字符串，则该命令将`value`在字符串的末尾附加。如果`key`不存在，它将被创建并设置为空字符串，因此 APPEND 在这种特殊情况下将与SET类似。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：追加操作后的字符串长度。

## 例子

redis> EXISTS mykey `(integer) 0` redis> APPEND mykey "Hello" `(integer) 5` redis> APPEND mykey " World" `(integer) 11` redis> GET mykey `"Hello World"`

## 模式：时间序列

APPEND 命令可用于创建固定大小样本列表的非常紧凑的表示，通常称为*时间序列*。每次新样品到达时，我们都可以使用命令将其存储起来

```javascript
APPEND timeseries "fixed-size sample"
```

访问时间序列中的单个元素并不难：

- 可以使用 STRLEN 来获取样本数量。

- GETRANGE 允许随机访问元素。如果我们的时间序列具有关联的时间信息，我们可以轻松实现二分查找，以便将GETRANGE 与 Redis 2.6 中提供的 Lua 脚本引擎结合起来。

- SETRANGE 可以用来覆盖现有的时间序列。

这种模式的局限性在于，我们被迫进入只有附加模式的操作模式，因为Redis目前缺少能够修剪字符串对象的命令，所以无法轻松地将时间序列缩减到给定大小。然而，以这种方式存储的时间序列的空间效率是显着的。

提示：可以根据当前的 Unix 时间切换到不同的密钥，这样每个密钥可能只有相对较少的采样数量，以避免处理非常大的密钥，并且使此模式更多友好可以分布在许多 Redis 实例中。

使用固定尺寸字符串采样传感器温度的示例（使用二进制格式在实际实现中更好）。

redis> APPEND ts "0043" `(integer) 4` redis> APPEND ts "0035" `(integer) 8` redis> GETRANGE ts 0 3 `"0043"` redis> GETRANGE ts 4 7 `"0035"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18