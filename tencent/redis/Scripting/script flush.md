# script flush

```javascript
SCRIPT FLUSH
```

**自2.6.0起可用。**

**时间复杂度：** O（N），其中N是缓存中的脚本数量

刷新 Lua 脚本缓存。

有关 Redis Lua 脚本的详细信息，请参阅 EVAL 文档。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18