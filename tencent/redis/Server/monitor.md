# monitor

```javascript
MONITOR
```

**自1.0.0起可用。**

MONITOR 是一个调试命令，用于回传 Redis 服务器处理的每个命令。它可以帮助理解数据库正在发生的事情。该命令可以通过`redis-cli`和通过使用`telnet`。

查看服务器处理的所有请求的能力对于在使用 Redis 作为数据库和分布式缓存系统时都能发现应用程序中的错误很有用。

```javascript
$ redis-cli monitor
1339518083.107412 [0 127.0.0.1:60866] "keys" "*"
1339518087.877697 [0 127.0.0.1:60866] "dbsize"
1339518090.420270 [0 127.0.0.1:60866] "set" "x" "6"
1339518096.506257 [0 127.0.0.1:60866] "get" "x"
1339518099.363765 [0 127.0.0.1:60866] "del" "x"
1339518100.544926 [0 127.0.0.1:60866] "get" "x"
```

使用`SIGINT`（ Ctrl-C ）停止正在运行的 MONITOR 流`redis-cli`。

```javascript
$ telnet localhost 6379
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
MONITOR
+OK
+1339518083.107412 [0 127.0.0.1:60866] "keys" "*"
+1339518087.877697 [0 127.0.0.1:60866] "dbsize"
+1339518090.420270 [0 127.0.0.1:60866] "set" "x" "6"
+1339518096.506257 [0 127.0.0.1:60866] "get" "x"
+1339518099.363765 [0 127.0.0.1:60866] "del" "x"
+1339518100.544926 [0 127.0.0.1:60866] "get" "x"
QUIT
+OK
Connection closed by foreign host.
```

手动发出 QUIT 命令以停止运行的 MONITOR 流`telnet`。

## MONITOR 未记录的命令

出于安全考虑，某些特殊的管理命令`CONFIG`不会登录到 MONITOR 输出中。

## 运行 MONITOR 的成本

由于 MONITOR 将**所有**命令传回，因此其使用需要付出代价。下面的（完全不科学的）基准数字说明了 MONITOR 的运行成本。

**未**运行 MONITOR 的基准测试结果：

```javascript
$ src/redis-benchmark -c 10 -n 100000 -q
PING_INLINE: 101936.80 requests per second
PING_BULK: 102880.66 requests per second
SET: 95419.85 requests per second
GET: 104275.29 requests per second
INCR: 93283.58 requests per second
```

基准测试结果**与**监视正在运行的（`redis-cli monitor > /dev/null`）：

```javascript
$ src/redis-benchmark -c 10 -n 100000 -q
PING_INLINE: 58479.53 requests per second
PING_BULK: 59136.61 requests per second
SET: 41823.50 requests per second
GET: 45330.91 requests per second
INCR: 41771.09 requests per second
```

在这种特殊情况下，运行单个 MONITOR 客户端可以将吞吐量降低50％以上。运行更多的 MONITOR 客户端将进一步降低吞吐量。

## 返回值

**非标准返回值**，只是将接收到的命令转储为无限流。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18