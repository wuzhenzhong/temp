# config get

```javascript
CONFIG GET parameter
```

**自2.0.0起可用。**

CONFIG GET 命令用于读取正在运行的 Redis 服务器的配置参数。在 Redis 2.4 中不支持所有的配置参数，而 Redis 2.6 可以使用此命令读取服务器的整个配置。

用于在运行时更改配置的对称命令是`CONFIG SET`。

CONFIG GET 接受一个参数，它是一个全局样式。所有与此参数匹配的配置参数都将报告为键值对的列表。例：

```javascript
redis> config get *max-*-entries*
1) "hash-max-zipmap-entries"
2) "512"
3) "list-max-ziplist-entries"
4) "512"
5) "set-max-intset-entries"
6) "512"
```

您可以通过输入`CONFIG GET *`打开的`redis-cli`提示来获取所有支持的配置参数的列表。

所有支持的参数都与 [redis.conf ](http://github.com/antirez/redis/raw/2.8/redis.conf)文件中使用的等效配置参数具有相同的含义，但具有以下重要区别：

- 在指定字节或其他数量的情况下，不可能使用`redis.conf`缩写形式（`10k`，`2gb`...等等），在配置指令的基本单元中，应将所有内容指定为格式良好的64位整数。

- save 参数是空格分隔整数的单个字符串。每一对整数代表一个秒/修改阈值。

例如，`redis.conf`看起来像什么：

```javascript
save 900 1
save 300 10
```

也就是说，如果数据集中至少有1次更改，则在900秒后保存，如果数据集中至少有10次更改，则在300秒后保存，将由 CONFIG GET 报告为“900 1 300 10”。

## 返回值

该命令的返回类型是 [Array 回复](https://redis.io/topics/protocol#array-reply)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18