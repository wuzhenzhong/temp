# client setname

```javascript
CLIENT SETNAME connection-name
```

**自2.6.9起可用。**

**时间复杂度：** O（1）

[纠错](javascript:;)

CLIENT SETNAME 命令为当前连接分配一个名称。

分配的名称显示在 CLIENT LIST 的输出中，以便可以识别执行给定连接的客户端。

例如，当使用 Redis 来实现队列时，消息的生产者和消费者可能希望根据其角色设置连接的名称。

如果不是 Redis 字符串类型的通常限制（512 MB），则可以分配的名称长度没有限制。但是，在连接名称中不能使用空格，因为这会违反 CLIENT LIST 答复的格式。

可以完全删除将其设置为空字符串的连接名称，这不是有效的连接名称，因为它可用于此特定目的。

连接名称可以使用 CLIENT GETNAME 进行检查。

每个新连接都没有分配名称。

提示：将名称设置为连接是调试由于应用程序中使用 Redis 中的错误导致的连接泄漏的好方法。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果连接名称设置成功。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18