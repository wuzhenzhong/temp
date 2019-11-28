# client reply

```javascript
CLIENT REPLY ON|OFF|SKIP
```

**自3.2起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

有时，客户端可以完全禁用来自 Redis 服务器的回复。例如，当客户端发送消息并忘记命令或执行大量数据加载时，或缓存不断传输新数据的上下文时。在这种情况下，使用服务器时间和带宽来回复客户端，这将被忽略，这被认为是浪费。

CLIENT REPLY 命令控制服务器是否回复客户端的命令。以下模式可用：

- `ON`。这是服务器返回每个命令的默认模式。

- `OFF`。在这种模式下，服务器不会回复客户端命令。

- `SKIP`。该模式在其之后立即跳过命令的回复。

## 返回值

使用任一`OFF`或`SKIP`子命令调用时，不会进行回复。当用`ON`以下方式调用

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply): `OK`.

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18