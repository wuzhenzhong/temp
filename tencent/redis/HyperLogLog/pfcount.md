# pfcount

```javascript
PFCOUNT key [key ...]
```

**自2.8.9起可用。**

**时间复杂度：** O（1）用单个密钥调用时的平均时间非常短。O（N），其中N是密钥的数量，当用多个密钥调用时，恒定时间大得多。

使用单个键调用时，返回由存储在指定变量中的 HyperLogLog 数据结构计算的近似基数，如果该变量不存在，则返回0。

使用多个键调用时，通过将存储在所提供的键中的 HyperLogLog 内部合并到临时 HyperLogLog 中，返回传递的 HyperlogLog 的联合的近似基数。

可以使用 HyperLogLog 数据结构，以便使用少量恒定内存（特别是每个 HyperLogLog 的12k字节（加上密钥本身的几个字节））对集合中的**唯一**元素进行计数。

观察到的集合的返回基数不是确切的，但是以0.81％的标准误差近似。

例如，为了计算一天中执行的所有唯一搜索查询的计数，程序需要在每次处理查询时调用 PFADD。可以随时使用 PFCOUNT 检索唯一查询的估计数量。

注意：由于调用此函数的副作用，HyperLogLog 可能会被修改，因为最后8个字节会将最新计算的基数编码为高速缓存目的。所以 PFCOUNT 在技术上是一个写命令。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- 通过PFADD观察到的独特元素的近似数量。

## 例子

redis> PFADD hll foo bar zap `(integer) 1` redis> PFADD hll zap zap zap `(integer) 0` redis> PFADD hll foo bar `(integer) 0` redis> PFCOUNT hll `(integer) 3` redis> PFADD some-other-hll 1 2 3 `(integer) 1` redis> PFCOUNT hll some-other-hll `(integer) 6`

## Performances

当使用单个键调用 PFCOUNT 时，即使理论上处理密集型 HyperLogLog 的时间很长，性能也非常好。这是可能的，因为 PFCOUNT 使用缓存来记住之前计算的基数，这很少发生变化，因为大多数 PFADD 操作不会更新任何寄存器。每秒可执行数百次操作。

当使用多个密钥调用 PFCOUNT 时，将执行 HyperLogLog 的即时合并，这很慢，而且联合的基数不能被缓存，所以当与多个密钥 PFCOUNT 一起使用时，可能需要一段时间毫秒数量级，并且不应该被滥用。

用户应该记住，该命令的单键和多键执行在语义上是不同的并且具有不同的性能。

## HyperLogLog representation

Redis HyperLogLog 使用双重表示法表示：适用于 HLL 计数少量元素（导致少量寄存器设置为非零值）的*稀疏*表示形式，以及适用于更高基数的*密集*表示形式。需要时，Redis 会自动从稀疏状态切换到密集状态。

稀疏表示使用经过优化的游程编码来有效地存储大量设置为零的寄存器。密集表示是一个12288字节的 Redis 字符串，用于存储16384个6位计数器。对双重表示的需求来自于使用12k（这是密集表示内存要求）对少量寄存器进行编码以获得更小的基数的事实，这是非常不理想的。

两种表示都以16字节的头部作为前缀，其中包含魔术，编码/版本字段以及计算出的缓存基数估计值，并以 little endian 格式存储（如果自 HyperLogLog 更新后估计无效，则最高有效位为1因为计算基数）。

作为 Redis 字符串的 HyperLogLog 可以使用GET进行检索并使用 SET 进行恢复。使用损坏的 HyperLogLog 调用 PFADD，PFCOUNT 或 PFMERGE 命令绝不是问题，它可能会返回随机值，但不会影响服务器的稳定性。大多数情况下，当破坏稀疏表示时，服务器会识别损坏并返回错误。

从处理器字长和字节序的观点来看，该表示是中性的，因此32位和64位处理器使用相同的表示法，即大字节或小字节。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18