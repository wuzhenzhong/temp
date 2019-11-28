# migrate

```javascript
MIGRATE host port key|"" destination-db timeout [COPY] [REPLACE] [KEYS key [key ...]]
```

**自2.6.0起可用。**

**时间复杂度：**该命令实际上在源实例中执行 DUMP + DEL，并在目标实例中执行 RESTORE。查看这些命令的页面以了解时间复杂性。此外还执行两个实例之间的O（N）数据传输。

将来自源 Redis 实例的密钥以原子方式传输到目标 Redis 实例。成功时，密钥从原始实例中删除，并保证存在于目标实例中。

该命令是原子性的，并在传输密钥所需的时间内阻塞两个实例，在任何给定时间，密钥似乎都会存在于给定实例中或其他实例中，除非发生超时错误。在3.2和更高版本中，通过传递空字符串（“”）作为键并添加 KEYS 子句，可以在 MIGRATE 的单个调用中对多个键进行流水线处理。

该命令在内部使用 DUMP 生成键值的序列化版本，并使用RESTORE 来合成目标实例中的键。源实例充当目标实例的客户端。如果目标实例向 RESTORE 命令返回 OK，则源实例将使用DEL删除该密钥。

超时指定与目标实例通信的任何时刻的最大空闲时间，以毫秒为单位。这意味着操作不需要在指定的毫秒数内完成，但是传输应该在没有阻塞超过指定的毫秒数的情况下进行。

MIGRATE 需要执行 I / O 操作并遵守指定的超时。当传输过程中发生 I / O 错误或达到超时时，操作中止，并`IOERR`返回特殊错误。发生这种情况时，以下两种情况是可能的：

- 密钥可能在两个实例上。

- 密钥可能只在源实例中。

发生超时时密钥不可能丢失，但如果发生超时错误，客户端调用MIGRATE 应检查密钥是否*也*存在于目标实例中，并相应采取相应措施。

当返回任何其他错误时（从开始`ERR`） MIGRATE 保证密钥仍然仅存在于初始实例中（除非同名的密钥*已经*存在于目标实例中）。

如果没有在源实例中迁移的密钥`NOKEY`返回。因为在正常情况下丢失的密钥是可能的，例如从失效到`NOKEY`不是错误。

## 使用一个命令调用迁移多个键

从Redis 3.0.6开始 MIGRATE 支持一种新的批量迁移模式，该模式使用流水线技术来迁移实例间的多个密钥，而不会产生往返时间延迟以及使用单个 MIGRATE 调用移动每个密钥时存在的其他开销。

为了启用此表单，使用了 KEYS 选项，并将常规*键*参数设置为空字符串。实际的键名将在 KEYS 参数本身之后提供，如下例所示：

当使用这种形式`NOKEY`时，只有在实例中没有任何键时才返回状态码，否则即使只有一个键存在，命令也会执行。

- `COPY` - 不要从本地实例中删除密钥。

- `REPLACE` - 替换远程实例上的现有密钥。

- keys - 如果键参数是一个空字符串，则该命令将迁移 KEYS 选项后面的所有键（有关详细信息，请参阅上面的部分）。

`COPY`并且`REPLACE`仅在3.0及以上版本中可用。KEYS可以从Redis 3.0.6开始使用。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)：该命令在成功时返回OK，或者`NOKEY`如果在源实例中未找到任何密钥。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  