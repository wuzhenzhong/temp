# multi

```javascript
MULTI
```

**自1.2.0起可用。**

标记[事务](https://redis.io/topics/transactions)块的开始。后续命令将排队等候使用EXEC进行原子执行。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：总是`OK`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18