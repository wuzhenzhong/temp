# command

```javascript
COMMAND
```

**自2.8.13起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N）其中 N 是 Redis 命令的总数

返回[Array回复来获取](https://redis.io/topics/protocol#array-reply)有关所有 Redis 命令的详细信息的。

集群客户端必须知道命令中的关键位置，以便命令可以转到匹配的实例，但 Redis 命令会在接受一个键，多个键或甚至由其他数据分隔的多个键之间变化。

您可以使用 COMMAND 缓存每个命令的命令和关键位置之间的映射，以确保将命令精确地路由到集群实例。

## 嵌套结果数组

每个顶级结果都包含六个嵌套结果。每个嵌套结果是：

- 命令名称

- 命令参数说明

- 嵌套[Array回复](https://redis.io/topics/protocol#array-reply)命令标志

- 第一个键在参数列表中的位置

- 最后一个键在参数列表中的位置

- 查找重复键的步数

### 命令名称

命令名称是以小写字符串形式返回的命令。

### 命令参数

| 1) 1) "get" 2) (integer) 2 3) 1) readonly 4) (integer) 1 5) (integer) 1 6) (integer) 1 | 1) 1) "mget" 2) (integer) -2 3) 1) readonly 4) (integer) 1 5) (integer) -1 6) (integer) 1 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
|                                                              |                                                              |

命令参数遵循一个简单的模式：

- 如果命令具有固定数量的必需参数，则为正数

- 如果命令具有所需参数的最小数量，则为负，但可能会有更多。

命令arity *包括*计数命令名称本身。

例子：

- GET arity是2，因为该命令只接受一个参数并始终具有该格式`GET _key_`。

- MGET arity是-2，因为该命令至少接受一个参数，但最多可以有一个无限制的数字：`MGET _key1_ [key2] [key3] ...`。

另请注意，使用 MGET 时，“最后一次键位置”的-1值表示键的列表可能具有无限长度。

### 标志

命令标志是包含一个或多个状态回复的[Array](https://redis.io/topics/protocol#array-reply)回复：

- *写* - 命令可能导致修改

- *只读* - 命令永远不会修改密钥

- *denyoom* -如果当前 OOM 拒绝命令

- *管理员* - 服务器管理命令

- *pubsub* - pubsub 相关的命令

- *noscript* - 从脚本中拒绝这个命令

- *随机* - 命令具有随机结果，对脚本有危险

- *sort_for_script* - 如果从脚本调用，则对输出进行排序

- *加载* - 在数据库加载时允许命令

- *stale* - 允许命令，而副本有陈旧的数据

- *skip_monitor* - 不要在 MONITOR 中显示此命令

- *询问* - 集群相关 - 即使导入也接受

- *fast* - 命令在恒定或 log（N）时间内运行。用于延迟监视。

- *可移动*键 - 键没有预先确定的位置。你必须自己发现钥匙。

### 可移动的键

```javascript
1) 1) "sort"
   2) (integer) -2
   3) 1) write
      2) denyoom
      3) movablekeys
   4) (integer) 1
   5) (integer) 1
   6) (integer) 1
```

一些 Redis 命令没有预定的关键位置。对于这些命令，将标志`movablekeys`添加到命令标志[数组答复中](https://redis.io/topics/protocol#array-reply)。您的 Redis Cluster 客户端需要解析标记的命令`movablekeys`以查找所有相关的关键位置。

完整的当前需要关键位置解析的命令列表：

- SORT - 可选`STORE`键，可选`BY`重量，可选 GET 键

- ZUNIONSTORE - 键停止时`WEIGHT`或`AGGREGATE`开始

- ZINTERSTORE - 键停止`WEIGHT`或`AGGREGATE`开始

- EVAL - 键在`numkeys`计数参数后停止

- EVALSHA - `numkeys`计数参数后键停止

另请参阅 COMMAND GETKEYS ，让您的 Redis 服务器告诉您在任何给定命令中的键。

### 参数列表中的第一个关键字

对于大多数命令，第一个键是位置1。位置0始终是命令名称本身。

### 参数列表中的最后一个键

Redis 命令通常接受一个密钥，两个密钥或无限数量的密钥。

如果一个命令接受一个键，则第一个键和最后一个键的位置是1。

如果一个命令接受两个键（例如 BRPOPLPUSH ，SMOVE ，RENAME ，...），则最后一个键位置是参数列表中最后一个键的位置。

如果一个命令接受无限数量的键，则最后一个键位置为-1。

### 步数

| 1) 1) "mset" 2) (integer) -3 3) 1) write 2) denyoom 4) (integer) 1 5) (integer) -1 6) (integer) 2 | 1) 1) "mget" 2) (integer) -2 3) 1) readonly 4) (integer) 1 5) (integer) -1 6) (integer) 1 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
|                                                              |                                                              |

关键步数允许我们找到格式为 MSET 的命令中的关键位置`MSET _key1_ _val1_ [key2] [val2] [key3] [val3]...`。

对于 MSET ，键位于其他位置，因此步长值为2。与上面的步长值仅为1的 MGET 相比较。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：命令详细信息的嵌套列表。命令以随机顺序返回。

## 例子

redis> COMMAND `1) 1) "zcard" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 2) 1) "object" 2) (integer) 3 3) 1) "readonly" 4) (integer) 2 5) (integer) 2 6) (integer) 2 3) 1) "unwatch" 2) (integer) 1 3) 1) "noscript" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 4) 1) "keys" 2) (integer) 2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 0 5) (integer) 0 6) (integer) 0 5) 1) "hdel" 2) (integer) -3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 6) 1) "echo" 2) (integer) 2 3) 1) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 7) 1) "pfselftest" 2) (integer) 1 3) 1) "admin" 4) (integer) 0 5) (integer) 0 6) (integer) 0 8) 1) "brpop" 2) (integer) -3 3) 1) "write" 2) "noscript" 4) (integer) 1 5) (integer) 1 6) (integer) 1 9) 1) "pttl" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 10) 1) "hincrbyfloat" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 11) 1) "slowlog" 2) (integer) -2 3) 1) "admin" 4) (integer) 0 5) (integer) 0 6) (integer) 0 12) 1) "hlen" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 13) 1) "hexists" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 14) 1) "lpush" 2) (integer) -3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 15) 1) "getset" 2) (integer) 3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 16) 1) "info" 2) (integer) -1 3) 1) "loading" 2) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 17) 1) "rpoplpush" 2) (integer) 3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 2 6) (integer) 1 18) 1) "zunionstore" 2) (integer) -4 3) 1) "write" 2) "denyoom" 3) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 19) 1) "lrem" 2) (integer) 4 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 20) 1) "rpush" 2) (integer) -3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 21) 1) "pexpireat" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 22) 1) "zrevrange" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 23) 1) "ttl" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 24) 1) "del" 2) (integer) -2 3) 1) "write" 4) (integer) 1 5) (integer) -1 6) (integer) 1 25) 1) "host:" 2) (integer) -1 3) 1) "loading" 2) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 26) 1) "rename" 2) (integer) 3 3) 1) "write" 4) (integer) 1 5) (integer) 2 6) (integer) 1 27) 1) "bgsave" 2) (integer) -1 3) 1) "admin" 4) (integer) 0 5) (integer) 0 6) (integer) 0 28) 1) "decrby" 2) (integer) 3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 29) 1) "sunion" 2) (integer) -2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) -1 6) (integer) 1 30) 1) "shutdown" 2) (integer) -1 3) 1) "admin" 2) "loading" 3) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 31) 1) "incrbyfloat" 2) (integer) 3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 32) 1) "pfcount" 2) (integer) -2 3) 1) "readonly" 4) (integer) 1 5) (integer) -1 6) (integer) 1 33) 1) "command" 2) (integer) 0 3) 1) "loading" 2) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 34) 1) "exists" 2) (integer) -2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) -1 6) (integer) 1 35) 1) "rpop" 2) (integer) 2 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 36) 1) "expireat" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 37) 1) "bitfield" 2) (integer) -2 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 38) 1) "lindex" 2) (integer) 3 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 39) 1) "zrank" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 40) 1) "module" 2) (integer) -2 3) 1) "admin" 2) "noscript" 4) (integer) 1 5) (integer) 1 6) (integer) 1 41) 1) "zinterstore" 2) (integer) -4 3) 1) "write" 2) "denyoom" 3) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 42) 1) "persist" 2) (integer) 2 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 43) 1) "getrange" 2) (integer) 4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 44) 1) "geodist" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 45) 1) "brpoplpush" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "noscript" 4) (integer) 1 5) (integer) 2 6) (integer) 1 46) 1) "zscore" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 47) 1) "georadiusbymember" 2) (integer) -5 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 48) 1) "zrevrangebylex" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 49) 1) "replconf" 2) (integer) -1 3) 1) "admin" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 50) 1) "sadd" 2) (integer) -3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 51) 1) "getbit" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 52) 1) "pfadd" 2) (integer) -2 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 53) 1) "zincrby" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 54) 1) "hgetall" 2) (integer) 2 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 55) 1) "zremrangebyscore" 2) (integer) 4 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 56) 1) "type" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 57) 1) "cluster" 2) (integer) -2 3) 1) "admin" 4) (integer) 0 5) (integer) 0 6) (integer) 0 58) 1) "zrange" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 59) 1) "debug" 2) (integer) -1 3) 1) "admin" 2) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 60) 1) "flushdb" 2) (integer) -1 3) 1) "write" 4) (integer) 0 5) (integer) 0 6) (integer) 0 61) 1) "bitcount" 2) (integer) -2 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 62) 1) "sunionstore" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 1 63) 1) "rpushx" 2) (integer) -3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 64) 1) "smove" 2) (integer) 4 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 2 6) (integer) 1 65) 1) "zrangebylex" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 66) 1) "multi" 2) (integer) 1 3) 1) "noscript" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 67) 1) "sdiff" 2) (integer) -2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) -1 6) (integer) 1 68) 1) "hscan" 2) (integer) -3 3) 1) "readonly" 2) "random" 4) (integer) 1 5) (integer) 1 6) (integer) 1 69) 1) "zrevrank" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 70) 1) "punsubscribe" 2) (integer) -1 3) 1) "pubsub" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 71) 1) "lset" 2) (integer) 4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 72) 1) "psetex" 2) (integer) 4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 73) 1) "hvals" 2) (integer) 2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) 1 6) (integer) 1 74) 1) "zrem" 2) (integer) -3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 75) 1) "smembers" 2) (integer) 2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) 1 6) (integer) 1 76) 1) "zscan" 2) (integer) -3 3) 1) "readonly" 2) "random" 4) (integer) 1 5) (integer) 1 6) (integer) 1 77) 1) "dbsize" 2) (integer) 1 3) 1) "readonly" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 78) 1) "sinterstore" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 1 79) 1) "lastsave" 2) (integer) 1 3) 1) "random" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 80) 1) "geoadd" 2) (integer) -5 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 81) 1) "evalsha" 2) (integer) -3 3) 1) "noscript" 2) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 82) 1) "scan" 2) (integer) -2 3) 1) "readonly" 2) "random" 4) (integer) 0 5) (integer) 0 6) (integer) 0 83) 1) "unsubscribe" 2) (integer) -1 3) 1) "pubsub" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 84) 1) "setex" 2) (integer) 4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 85) 1) "scard" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 86) 1) "ping" 2) (integer) -1 3) 1) "stale" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 87) 1) "bitpos" 2) (integer) -3 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 88) 1) "psubscribe" 2) (integer) -2 3) 1) "pubsub" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 89) 1) "role" 2) (integer) 1 3) 1) "noscript" 2) "loading" 3) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 90) 1) "wait" 2) (integer) 3 3) 1) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 91) 1) "config" 2) (integer) -2 3) 1) "admin" 2) "loading" 3) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 92) 1) "publish" 2) (integer) 3 3) 1) "pubsub" 2) "loading" 3) "stale" 4) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 93) 1) "sdiffstore" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 1 94) 1) "lrange" 2) (integer) 4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 95) 1) "hsetnx" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 96) 1) "asking" 2) (integer) 1 3) 1) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 97) 1) "decr" 2) (integer) 2 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 98) 1) "client" 2) (integer) -2 3) 1) "admin" 2) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 99) 1) "hstrlen" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 100) 1) "linsert" 2) (integer) 5 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 101) 1) "swapdb" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 102) 1) "spop" 2) (integer) -2 3) 1) "write" 2) "random" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 103) 1) "subscribe" 2) (integer) -2 3) 1) "pubsub" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 104) 1) "lpushx" 2) (integer) -3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 105) 1) "ltrim" 2) (integer) 4 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 106) 1) "migrate" 2) (integer) -6 3) 1) "write" 2) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 107) 1) "llen" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 108) 1) "zlexcount" 2) (integer) 4 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 109) 1) "psync" 2) (integer) 3 3) 1) "readonly" 2) "admin" 3) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 110) 1) "restore-asking" 2) (integer) -4 3) 1) "write" 2) "denyoom" 3) "asking" 4) (integer) 1 5) (integer) 1 6) (integer) 1 111) 1) "save" 2) (integer) 1 3) 1) "admin" 2) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 112) 1) "latency" 2) (integer) -2 3) 1) "admin" 2) "noscript" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 113) 1) "setnx" 2) (integer) 3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 114) 1) "auth" 2) (integer) 2 3) 1) "noscript" 2) "loading" 3) "stale" 4) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 115) 1) "hmget" 2) (integer) -3 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 116) 1) "sinter" 2) (integer) -2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) -1 6) (integer) 1 117) 1) "watch" 2) (integer) -2 3) 1) "noscript" 2) "fast" 4) (integer) 1 5) (integer) -1 6) (integer) 1 118) 1) "strlen" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 119) 1) "sync" 2) (integer) 1 3) 1) "readonly" 2) "admin" 3) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 120) 1) "bitop" 2) (integer) -4 3) 1) "write" 2) "denyoom" 4) (integer) 2 5) (integer) -1 6) (integer) 1 121) 1) "zrangebyscore" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 122) 1) "geohash" 2) (integer) -2 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 123) 1) "msetnx" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 2 124) 1) "hmset" 2) (integer) -4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 125) 1) "touch" 2) (integer) -2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 126) 1) "post" 2) (integer) -1 3) 1) "loading" 2) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 127) 1) "pfmerge" 2) (integer) -2 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 1 128) 1) "readwrite" 2) (integer) 1 3) 1) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 129) 1) "sort" 2) (integer) -2 3) 1) "write" 2) "denyoom" 3) "movablekeys" 4) (integer) 1 5) (integer) 1 6) (integer) 1 130) 1) "monitor" 2) (integer) 1 3) 1) "admin" 2) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 131) 1) "randomkey" 2) (integer) 1 3) 1) "readonly" 2) "random" 4) (integer) 0 5) (integer) 0 6) (integer) 0 132) 1) "incr" 2) (integer) 2 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 133) 1) "geopos" 2) (integer) -2 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 134) 1) "mget" 2) (integer) -2 3) 1) "readonly" 4) (integer) 1 5) (integer) -1 6) (integer) 1 135) 1) "hincrby" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 136) 1) "srandmember" 2) (integer) -2 3) 1) "readonly" 2) "random" 4) (integer) 1 5) (integer) 1 6) (integer) 1 137) 1) "zremrangebyrank" 2) (integer) 4 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 138) 1) "script" 2) (integer) -2 3) 1) "noscript" 4) (integer) 0 5) (integer) 0 6) (integer) 0 139) 1) "srem" 2) (integer) -3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 140) 1) "setrange" 2) (integer) 4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 141) 1) "mset" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) -1 6) (integer) 2 142) 1) "flushall" 2) (integer) -1 3) 1) "write" 4) (integer) 0 5) (integer) 0 6) (integer) 0 143) 1) "blpop" 2) (integer) -3 3) 1) "write" 2) "noscript" 4) (integer) 1 5) (integer) -2 6) (integer) 1 144) 1) "renamenx" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 2 6) (integer) 1 145) 1) "select" 2) (integer) 2 3) 1) "loading" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 146) 1) "bgrewriteaof" 2) (integer) 1 3) 1) "admin" 4) (integer) 0 5) (integer) 0 6) (integer) 0 147) 1) "zcount" 2) (integer) 4 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 148) 1) "substr" 2) (integer) 4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 149) 1) "georadius" 2) (integer) -6 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 150) 1) "sismember" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 151) 1) "incrby" 2) (integer) 3 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 152) 1) "hget" 2) (integer) 3 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 153) 1) "zrevrangebyscore" 2) (integer) -4 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 154) 1) "setbit" 2) (integer) 4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 155) 1) "time" 2) (integer) 1 3) 1) "random" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 156) 1) "slaveof" 2) (integer) 3 3) 1) "admin" 2) "noscript" 3) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 157) 1) "hset" 2) (integer) 4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 158) 1) "dump" 2) (integer) 2 3) 1) "readonly" 4) (integer) 1 5) (integer) 1 6) (integer) 1 159) 1) "move" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 160) 1) "sscan" 2) (integer) -3 3) 1) "readonly" 2) "random" 4) (integer) 1 5) (integer) 1 6) (integer) 1 161) 1) "append" 2) (integer) 3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 162) 1) "memory" 2) (integer) -2 3) 1) "readonly" 4) (integer) 0 5) (integer) 0 6) (integer) 0 163) 1) "discard" 2) (integer) 1 3) 1) "noscript" 2) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 164) 1) "lpop" 2) (integer) 2 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 165) 1) "pexpire" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 166) 1) "pfdebug" 2) (integer) -3 3) 1) "write" 4) (integer) 0 5) (integer) 0 6) (integer) 0 167) 1) "readonly" 2) (integer) 1 3) 1) "fast" 4) (integer) 0 5) (integer) 0 6) (integer) 0 168) 1) "get" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 169) 1) "zadd" 2) (integer) -4 3) 1) "write" 2) "denyoom" 3) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 170) 1) "hkeys" 2) (integer) 2 3) 1) "readonly" 2) "sort_for_script" 4) (integer) 1 5) (integer) 1 6) (integer) 1 171) 1) "restore" 2) (integer) -4 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 172) 1) "exec" 2) (integer) 1 3) 1) "noscript" 2) "skip_monitor" 4) (integer) 0 5) (integer) 0 6) (integer) 0 173) 1) "eval" 2) (integer) -3 3) 1) "noscript" 2) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 174) 1) "set" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 175) 1) "expire" 2) (integer) 3 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 176) 1) "zremrangebylex" 2) (integer) 4 3) 1) "write" 4) (integer) 1 5) (integer) 1 6) (integer) 1 177) 1) "unlink" 2) (integer) -2 3) 1) "write" 2) "fast" 4) (integer) 1 5) (integer) -1 6) (integer) 1 178) 1) "pubsub" 2) (integer) -2 3) 1) "pubsub" 2) "random" 3) "loading" 4) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18