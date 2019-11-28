# command info

```javascript
COMMAND INFO command-name [command-name ...]
```

**自2.8.13起可用。**

**时间复杂度：**当 N 是要查找的命令数时，O（N）

返回有关多个 Redis 命令的详细信息的 [Array 回复](https://redis.io/topics/protocol#array-reply)。

除 COMMAND 之外，您可以指定返回哪些命令。

如果您要求详细了解不存在的命令，它们的返回位置将为零。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：命令详细信息的嵌套列表。

## 例子

redis> COMMAND INFO get set eval `1) 1) "get" 2) (integer) 2 3) 1) "readonly" 2) "fast" 4) (integer) 1 5) (integer) 1 6) (integer) 1 2) 1) "set" 2) (integer) -3 3) 1) "write" 2) "denyoom" 4) (integer) 1 5) (integer) 1 6) (integer) 1 3) 1) "eval" 2) (integer) -3 3) 1) "noscript" 2) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0` redis> COMMAND INFO foo evalsha config bar `1) (nil) 2) 1) "evalsha" 2) (integer) -3 3) 1) "noscript" 2) "movablekeys" 4) (integer) 0 5) (integer) 0 6) (integer) 0 3) 1) "config" 2) (integer) -2 3) 1) "admin" 2) "loading" 3) "stale" 4) (integer) 0 5) (integer) 0 6) (integer) 0 4) (nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18