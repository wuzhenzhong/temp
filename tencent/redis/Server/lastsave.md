# lastsave

```javascript
LASTSAVE
```

**自1.0.0起可用。**

返回成功执行的最后一次数据库保存的 UNIX TIME 。客户端可以检查 BGSAVE 命令是否成功读取 LASTSAVE 值，然后发出 BGSAVE 命令并每隔 N 秒定期检查 LASTSAVE 是否更改。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：UNIX 时间戳。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18