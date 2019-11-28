# client getname

```javascript
CLIENT GETNAME
```

**自2.6.9起可用。**

**时间复杂度：** O（1）

CLIENT GETNAME 返回由 CLIENT SETNAME 设置的当前连接的名称。由于每个新连接都是在没有关联名称的情况下启动的，因此如果未分配名称，则会返回空批量答复。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：连接名称，如果未设置名称，则为空批量回复。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18