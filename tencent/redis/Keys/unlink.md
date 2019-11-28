# unlink

```javascript
UNLINK key [key ...]
```

**自4.0.0起可用。**

**时间复杂度：** O（1）每个键被删除而不管其大小。然后，命令O（N）在另一个线程中工作以回收内存，其中N是分配组成的已删除对象的分配数量。

该命令与 DEL 非常类似：它删除指定的键。就像 DEL 键一样，如果它不存在，它将被忽略。但是，该命令在不同的线程中执行实际的内存回收，因此它不会被阻塞，而 DEL 是。这是命令名称的来源：该命令只是**将**键与键空间**断开**连接。实际的删除将在晚些时候异步完成。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：未链接的键数。

## 例子

redis> SET key1 "Hello" `"OK"` redis> SET key2 "World" `"OK"` redis> UNLINK key1 key2 key3 `ERR Unknown or disabled command 'UNLINK'`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  