# config set

```javascript
CONFIG SET parameter value
```

**自2.0.0起可用。**

CONFIG SET 命令用于在运行时重新配置服务器，而无需重新启动 Redis 。您可以使用此命令更改这两个微不足道的参数或从一个持久选项切换到另一个持久选项。

CONFIG SET 支持的配置参数列表可以通过发出`CONFIG GET *`命令获得，即用于获取有关正在运行的 Redis 实例的配置信息的对称命令。

使用 CONFIG SET 设置的所有配置参数都由 Redis 立即加载，并将在下一个执行的命令开始生效。

所有支持的参数都与 [redis.conf ](http://github.com/antirez/redis/raw/2.8/redis.conf)文件中使用的等效配置参数具有相同的含义，但具有以下重要区别：

- 在指定字节或其他数量的选项中，不可能使用`redis.conf`缩写形式（`10k`，`2gb`...等等），在配置的基本单元中，所有内容都应指定为格式良好的64位整数指示。但是，自 Redis 版本3.0或更高版本以来，可以使用带有内存单元的 CONFIG SET 作为`maxmemory`客户端输出缓冲区和复制积压大小。

- save 参数是空格分隔整数的单个字符串。每一对整数代表一个秒/修改阈值。

例如，`redis.conf`看起来像什么：

```javascript
save 900 1
save 300 10
```

也就是说，如果数据集至少有1次更改，则在900秒后保存，如果数据集至少有10次更改，则在300秒后保存，应使用`CONFIG SET SAVE "900 1 300 10"`。

可以使用 CONFIG SET 命令将持久性从 RDB 快照切换到仅附加文件（以及其他方式）。有关如何执行此操作的更多信息，请检查[持久性页面](https://redis.io/topics/persistence)。

一般来说，你应该知道的是，将`appendonly`参数设置为`yes`将启动一个后台进程来保存初始仅支持附加文件（从内存数据集中获取），并将所有后续命令添加到仅附加文件中，因此从一开始就开始启用 AOF 的 Redis 服务器获得完全相同的效果。

如果你愿意，你可以同时启用 AOF 和 RDB 快照，这两个选项不是互斥的。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`当配置设置正确时。否则会返回错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18