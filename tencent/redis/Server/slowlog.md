# slowlog

```javascript
SLOWLOG subcommand [argument]
```

**自2.2.12起可用。**

该命令用于读取和重置Redis慢速查询日志。

[纠错](javascript:;)

## Redis缓慢的日志概述

Redis Slow Log是一个记录查询超过指定执行时间的系统。执行时间不包括I / O操作，如与客户端交谈，发送回复等等，但仅仅是实际执行命令所需的时间（这是执行命令的唯一阶段，其中线程被阻止并且不能同时服务于其他请求）。

您可以使用两个参数配置慢日志：*slowlog-log-slow-than* 告知 Redis 为了记录命令，执行时间（微秒）超过了多少。请注意，负数将禁用慢日志，而值为零将强制记录每条命令。*slowlog-max-len* 是慢日志的长度。最小值为零。当记录新的命令并且慢日志已经处于其最大长度时，为了留出空间，将最老的命令从记录的命令队列中移除。

配置可以通过编辑完成，也可以`redis.conf`在服务器运行时使用CONFIG GET和CONFIG SET命令来完成。

## 读慢日志

慢日志在内存中累积，因此没有写入关于慢命令执行信息的文件。这使得日志记录非常快，可以启用所有命令的日志记录（将 *slowlog-log-slow-* config 配置参数设置为零），同时影响较小。

要读取慢日志，使用 **SLOWLOG GET** 命令，该命令将返回慢日志中的每个条目。可以仅返回N个最近的条目，并将其他参数传递给该命令（例如 **SLOWLOG GET 10**）。

请注意，为了读取慢日志输出，您需要使用 redis-cli 的最新版本，因为它使用了以前在 redis-cli 中执行的一些协议功能（深度嵌套的多批量响应）。

## 输出格式

```javascript
redis 127.0.0.1:6379> slowlog get 2
1) 1) (integer) 14
   2) (integer) 1309448221
   3) (integer) 15
   4) 1) "ping"
2) 1) (integer) 13
   2) (integer) 1309448128
   3) (integer) 30
   4) 1) "slowlog"
      2) "get"
      3) "100"
```

还有只有 Redis 4.0或更高版本才能发布的可选字段：

```javascript
5) "127.0.0.1:58217"
6) "worker-123"
```

每个条目由四个（或以 Redis 4.0开头的六个）字段组成：

- 每个慢日志条目的唯一渐进标识符。

- 处理记录的命令的 UNIX 时间戳。

- 执行所需的时间量，以微秒为单位。

- 组成命令参数的数组。

- 客户端 IP 地址和端口（仅限4.0）。

- 客户端名称（如果通过 CLIENT SETNAME 命令设置）（仅限4.0）。

该条目的唯一 ID 可用于避免多次处理缓慢的日志条目（例如，您可能有一个脚本为每个新的慢日志条目发送电子邮件警报）。

在 Redis 服务器执行过程中，ID 永远不会被重置，只有服务器重启才会重置它。

## 获取慢日志的当前长度

使用命令 **SLOWLOG LEN** 可以获得慢日志的长度。

## 重置慢日志。

您可以使用 **SLOWLOG RESET** 命令重置慢日志。一旦删除，信息将永远丢失。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18