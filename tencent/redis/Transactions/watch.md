# watch

```javascript
WATCH key [key ...]
```

**自2.2.0起可用。**

**时间复杂度：** O（1）为每个密钥。

标记要监视的给定密钥以进行有条件的[事务处理](https://redis.io/topics/transactions)。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：总是`OK`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18