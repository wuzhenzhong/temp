# bgsave

```javascript
BGSAVE
```

**自1.0.0起可用。**

[纠错](javascript:;)

将数据库保存在后台。OK 代码立即返回。Redis 实施叉形指令，父级继续为客户服务，子代将数据库保存在磁盘上，然后退出。客户端可以使用 LASTSAVE 命令检查操作是否成功。

有关详细信息，请参阅[持久性文档](https://redis.io/topics/persistence)。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18