# readonly

```javascript
READONLY
```

**自3.0.0起可用。**

**时间复杂度：** O（1）

启用读取查询以连接到 Redis 群集从属节点。

通常，从属节点会将客户端重定向到授权主服务器以获取给定命令中涉及的散列槽，但客户端可以使用从服务器来使用 READONLY 命令来扩展读取。

READONLY 告知 Redis 集群从节点客户机愿意读取可能过时的数据，并且对运行写查询不感兴趣。

当连接处于只读模式时，只有当操作涉及从属主节点未提供的密钥时，群集才会向客户端发送重定向。这可能是因为：

\1. 客户端发送了一个关于散列槽的命令，散列槽从来没有被这个从设备的主设备所服务

\2. 该集群已重新配置（例如重新绑定），从属服务器不再能够为给定的哈希槽提供命令。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18