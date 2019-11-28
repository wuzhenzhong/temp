# exec

```javascript
EXEC
```

**自1.2.0起可用。**

执行[事务中](https://redis.io/topics/transactions)以前排队的所有命令，并将连接状态恢复正常。

当使用 WATCH 时，只有在观看的键没有被修改时，EXEC 才会执行命令，允许[检查和设置机制](https://redis.io/topics/transactions#cas)。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：每个元素都是对原子事务中每个命令的回复。

当使用 WATCH 时，如果执行被中止，EXEC 可以返回一个[空回复](https://redis.io/topics/protocol#nil-reply)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18