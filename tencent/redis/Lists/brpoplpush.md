# brpoplpush

```javascript
BRPOPLPUSH source destination timeout
```

**自2.2.0起可用。**

**时间复杂度：** O（1）

BRPOPLPUSH是RPOPLPUSH的阻止变体。当`source`包含元素时，该命令的行为与RPOPLPUSH完全相同。在MULTI / EXEC块内使用时，该命令的行为与RPOPLPUSH完全相同。当它`source`为空时，Redis将阻止连接，直到另一个客户端推送它或直到`timeout`达到。`timeout`零值 可以用来无限地阻止。

有关更多信息，请参阅RPOPLPUSH。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：从中弹出`source`并推送到的元素`destination`。如果`timeout`达到，则返回[空回复](https://redis.io/topics/protocol#nil-reply)。

## 模式：可靠的队列

请参阅RPOPLPUSH文档中的模式描述。

## 模式：循环列表

请参阅RPOPLPUSH文档中的模式描述。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18