# incr

```javascript
INCR key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

将存储的数字`key`加1。如果密钥不存在，则`0`在执行操作之前将其设置为。如果密钥包含错误类型的值或包含无法表示为整数的字符串，则会返回错误。该操作仅限于64位有符号整数。

**注意**：这是一个字符串操作，因为Redis没有专用的整数类型。存储在密钥中的字符串被解释为一个以10为底的**64位有符号整数**来执行操作。

Redis 以整数表示形式存储整数，因此对于实际上包含整数的字符串值，不存在用于存储整数的字符串表示形式的开销。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：`key`增量后的值

## 例子

redis> SET mykey "10" `"OK"` redis> INCR mykey `(integer) 11` redis> GET mykey `"11"`

## 模式：计数器

计数器模式是使用 Redis 原子增量操作可以做的最明显的事情。这个想法只是在每次操作发生时发送 INCR 命令给 Redis。例如，在一个 Web 应用程序中，我们可能想知道这个用户一年中每天都做了多少页面浏览。

为此，Web 应用程序可以在每次用户执行页面视图时简单地增加一个键，创建连接用户 ID 的键名和代表当前日期的字符串。

这种简单模式可以通过多种方式进行扩展：

- 可以在每个页面视图中一起使用 INCR 和 EXPIRE，使计数器只计算最近 N 个分页少于指定秒数的页面视图。

- 客户端可以使用 GETSET 来自动获取当前计数器值并将其重置为零。

- 使用其他原子增量/减量命令（如 DECR 或 INCRBY），可以根据用户执行的操作来处理可能变大或变小的值。想象一下在线游戏中不同用户的分数。

## 模式：限速器

速率限制器模式是一个特殊的计数器，用于限制执行操作的速率。这种模式的经典实现涉及限制可以针对公共 API 执行的请求的数量。

我们使用 INCR 提供了这种模式的两种实现，我们假设要解决的问题是将 API 调用的数量限制为*每IP地址每秒*最多*10个请求*。

## 模式：限速器1

这种模式的更简单直接的实现如下：

```javascript
FUNCTION LIMIT_API_CALL(ip)
ts = CURRENT_UNIX_TIME()
keyname = ip+":"+ts
current = GET(keyname)
IF current != NULL AND current > 10 THEN
    ERROR "too many requests per second"
ELSE
    MULTI
        INCR(keyname,1)
        EXPIRE(keyname,10)
    EXEC
    PERFORM_API_CALL()
END
```

基本上我们有每个IP的柜台，每隔一秒钟。但是这个计数器总是递增的，设置10秒过期，以便在当前秒不同时自动将它们由 Redis移除。

请注意使用 MULTI 和 EXEC，以确保我们在每次 API 调用时都会递增并设置到期。

## 模式：限速器2

另一种实现方法是使用单个计数器，但要在没有竞争条件的情况下进行正确的设置会更复杂一些。我们将研究不同的变体。

```javascript
FUNCTION LIMIT_API_CALL(ip):
current = GET(ip)
IF current != NULL AND current > 10 THEN
    ERROR "too many requests per second"
ELSE
    value = INCR(ip)
    IF value == 1 THEN
        EXPIRE(ip,1)
    END
    PERFORM_API_CALL()
END
```

计数器的创建方式只能在当前秒的第一次请求开始后才能存活一秒。如果在同一秒内有超过10个请求，则计数器将达到大于10的值，否则将过期并从0开始。

**在上面的代码中有一个竞争条件**。如果由于某种原因，客户端执行INCR 命令但不执行 EXPIRE，密钥将被泄露，直到我们再次看到相同的IP地址。

这可以很容易地解决，将带有可选 EXPIRE 的 INCR 转换为使用EVAL 命令发送的 Lua 脚本（仅在 Redis 版本2.6以后可用）。

```javascript
local current
current = redis.call("incr",KEYS[1])
if tonumber(current) == 1 then
    redis.call("expire",KEYS[1],1)
end
```

有一种不使用脚本解决此问题的方法，但使用 Redis 列表而不是计数器。实现更复杂，使用更多高级功能，但具有记住当前正在执行API调用的客户端的IP地址的优点，根据应用程序的不同，该地址可能有用或无用。

```javascript
FUNCTION LIMIT_API_CALL(ip)
current = LLEN(ip)
IF current > 10 THEN
    ERROR "too many requests per second"
ELSE
    IF EXISTS(ip) == FALSE
        MULTI
            RPUSH(ip,ip)
            EXPIRE(ip,1)
        EXEC
    ELSE
        RPUSHX(ip,ip)
    END
    PERFORM_API_CALL()
END
```

如果密钥已经存在，RPUSHX 命令只会推送元素。

请注意，我们在这里有一场竞赛，但这不是问题：在我们在MULTI / EXEC 块中创建它之前，EXISTS 可能会返回 false，但可能会由另一个客户端创建密钥。然而，这种竞争只会在极少数情况下错过API调用，所以速率限制仍然可以正常工作。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18