# rpoplpush

```javascript
RPOPLPUSH source destination
```

**自1.2.0起可用。**

**时间复杂度：** O（1）

原子返回并移除存储在列表中的列表的最后一个元素（尾部）`source`，并按下存储在列表中第一个元素（头部）的元素`destination`。

例如：考虑`source`拿着清单`a,b,c`，然后`destination`拿着清单`x,y,z`。执行RPOPLPUSH导致`source`持有`a,b`和`destination`持有`c,x,y,z`。

如果`source`不存在，`nil`则返回该值并且不执行任何操作。如果`source`和`destination`相同，该操作相当于从列表中删除最后一个元素，并将其作为列表的第一个元素推入，因此可以将其视为列表旋转命令。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：正在弹出和推送的元素。

## 例子

redis> RPUSH mylist "one" `(integer) 1` redis> RPUSH mylist "two" `(integer) 2` redis> RPUSH mylist "three" `(integer) 3` redis> RPOPLPUSH mylist myotherlist `"three"` redis> LRANGE mylist 0 -1 `1) "one" 2) "two"` redis> LRANGE myotherlist 0 -1 `1) "three"`

## 模式：可靠的队列

Redis通常用作消息传递服务器来实现后台作业或其他类型的消息传递任务的处理。通常可以通过将值推入生产者端的列表中，并使用RPOP（使用轮询）在消费者端等待此值，或通过阻止操作为客户端提供更好的服务来实现BRPOP，从而获得简单的队列形式。

然而，在这种情况下，所获得的队列是不*可靠的，*因为可能会丢失消息，例如在存在网络问题的情况下或消费者在收到消息之后崩溃但仍然要处理的情况下。

RPOPLPUSH（或阻止变体的BRPOPLPUSH）提供了一种避免此问题的方法：消费者提取消息并同时将其推入处理列表。一旦消息已被处理，它将使用LREM命令来从处理列表中删除消息。

额外的客户端可能会监视处理列表中保留的项目太多时间，并且会在需要时再次将这些超时项目推入队列中。

## 模式：循环列表

使用具有相同源和目标密钥的RPOPLPUSH，客户端可以使用单个LRANGE操作在O（N）中一个接一个地访问N元素列表中的所有元素，而无需使用单个LRANGE操作将完整列表从服务器传输到客户端。

即使有以下两个条件，上述模式仍然有效：

- 有多个客户端轮换列表：他们将获取不同的元素，直到列表中的所有元素都被访问，并且进程重新启动。

- 即使其他客户在列表末尾积极推出新项目。

上述内容使得实现一套系统必须由N名工作人员尽可能快地处理一套物品变得非常简单。一个例子是一个监控系统，它必须使用大量的并行工作人员来检查一组网站是否可以访问，并尽可能缩短延迟。

请注意，这种工作者的实现是可升级且可靠的，因为即使消息丢失，项目仍然在队列中，并且将在下一次迭代中处理。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18