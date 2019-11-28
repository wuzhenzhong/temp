# readwrite

```javascript
READWRITE
```

**自3.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

禁用与 Redis 集群从属节点的连接的读取查询。

针对 Redis 集群从属节点的读取查询默认情况下处于禁用状态，但您可以使用 READONLY 命令以每个连接为基础更改此行为。READWRITE 命令将连接的只读模式标志重新设置为 readwrite。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18