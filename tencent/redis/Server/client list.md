# client list

```javascript
CLIENT LIST
```

**自2.4.0起可用。**

**时间复杂度：** O（N）其中 N 是客户端连接数

CLIENT LIST 命令以大多数人可读的格式返回有关客户端连接服务器的信息和统计信息。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：唯一字符串，格式如下：

- 每行一个客户端连接（由 LF 分隔）

- 每行由`property=value`由空格字符分隔的一系列字段组成。

这里是这些字段的含义：

- `id`：唯一的64位客户端 ID（在 Redis 2.8.12 中引入）。

- `addr`：客户端的地址/端口

- `fd`：与套接字对应的文件描述符

- `age`：连接的总持续时间，以秒为单位

- `idle`：以秒为单位的连接空闲时间

- `flags`：客户端标志（见下文）

- `db`：当前数据库 ID

- `sub`：频道订阅的数量

- `psub`：模式匹配订阅的数量

- `multi`：MULTI / EXEC 上下文中的命令数量

- `qbuf`：查询缓冲区长度（0表示没有待处理的查询）

- `qbuf-free`：查询缓冲区的空闲空间（0表示缓冲区已满）

- `obl`：输出缓冲区长度

- `oll`：输出列表长度（当缓冲区已满时，回复在此列表中排队）

- `omem`：输出缓冲区内存使用量

- `events`：文件描述符事件（见下文）

- `cmd`：播放最后的命令

客户端标志可以是以下的组合：

```javascript
O: the client is a slave in MONITOR mode
S: the client is a normal slave server
M: the client is a master
x: the client is in a MULTI/EXEC context
b: the client is waiting in a blocking operation
i: the client is waiting for a VM I/O (deprecated)
d: a watched keys has been modified - EXEC will fail
c: connection to be closed after writing entire reply
u: the client is unblocked
U: the client is connected via a Unix domain socket
r: the client is in readonly mode against a cluster node
A: connection to be closed ASAP
N: no specific flag set
```

文件描述符事件可以是：

```javascript
r: the client socket is readable (event loop)
w: the client socket is writable (event loop)
```

## 注意

定期添加新的字段用于调试目的。将来有些可能会被删除。使用此命令的版本安全的 Redis 客户端应相应地解析输出（即正确处理丢失的字段，跳过未知字段）。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18