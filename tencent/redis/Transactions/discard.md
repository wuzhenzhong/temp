# discard

```javascript
DISCARD
```

**自2.0.0起可用。**

[纠错](javascript:;)

刷新[事务](https://redis.io/topics/transactions)先前排队的所有命令，并将连接状态恢复正常。

如果使用 WATCH，则 DISCARD 将查看连接监视的所有按键。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：总是`OK`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18