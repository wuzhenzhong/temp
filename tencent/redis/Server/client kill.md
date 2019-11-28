# client kill

```javascript
CLIENT KILL [ip:port] [ID client-id] [TYPE normal|master|slave|pubsub] [ADDR ip:port] [SKIPME yes/no]
```

**自2.4.0起可用。**

**时间复杂度：** O（N）其中 N 是客户端连接数

[纠错](javascript:;)

CLIENT KILL 命令关闭给定的客户端连接。截至 Redis 2.8.11 ，可以仅通过客户端地址关闭连接，使用以下格式：

```javascript
CLIENT KILL addr:port
```

在`ip:port`应该与由客户端列表命令（返回线`addr`字段）。

但是，从 Redis 2.8.12 或更高版本开始，该命令接受以下格式：

```javascript
CLIENT KILL <filter> <value> ... ... <filter> <value>
```

使用新形式，可以通过不同的属性处理客户端，而不是仅仅通过地址来处理客户端。以下过滤器可用：

- `CLIENT KILL ADDR ip:port`。这与旧的三参数行为完全一样。

- `CLIENT KILL ID client-id`。允许通过其唯一`ID`字段来终止客户端，该字段是从 Redis 2.8.12 开始的 CLIENT LIST 命令中引入的。

- `CLIENT KILL TYPE type`，其中*类型*是之一`normal`，`master`，`slave`和`pubsub`（的`master`类型可从V3.2）。这将关闭指定类中**所有客户端**的连接。请注意，被锁定到 MONITOR 命令中的客户端被认为属于`normal`该类。

- `CLIENT KILL SKIPME yes/no`。默认情况下，这个选项被设置为`yes`，也就是说，调用该命令的客户端不会被`no`终止，但是设置该选项的效果也会导致调用该命令的客户端被终止。

可以同时提供多个过滤器。该命令将通过逻辑 AND 处理多个过滤器。例如：

```javascript
CLIENT KILL addr 127.0.0.1:6379 type slave
```

是有效的，只会处理具有指定地址的从站。这种包含多个过滤器的格式目前很少有用。

当使用新表单时，命令不再返回`OK`或发生错误，而是终止的客户机数量，可能为零。

## CLIENT KILL 和 Redis Sentinel

最近版本的 Redis Sentinel（ Redis 2.8.12 或更高版本）使用 CLIENT KILL 为了在重新配置实例时终止客户端，以强制客户端再次与一个 Sentinel 执行握手并更新其配置。

## 注

由于 Redis 的单线程性质，在执行命令时无法终止客户端连接。从客户的角度来看，连接永远不会在执行命令的过程中关闭。但是，客户端会注意到，只有在发送下一个命令时才会关闭连接（并导致网络错误）。

## 返回值

当用三个参数格式调用时：

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果连接存在并且已关闭

当用过滤器/值格式调用时：

[整数回复](https://redis.io/topics/protocol#integer-reply)：已解决的客户数量。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18