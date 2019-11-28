# script load

```javascript
SCRIPT LOAD script
```

**自2.6.0起可用。**

**时间复杂度：** O（N），其中N是脚本主体的字节长度。

将脚本加载到脚本缓存中，而不执行它。在将指定的命令加载到脚本缓存中之后，将使用 EVALSHA 和脚本的正确 SHA1 摘要来调用它，就像在第一次成功调用 EVAL 之后一样。

该脚本被保证永远留在脚本缓存中（除非`SCRIPT FLUSH`被调用）。

即使该脚本已存在于脚本缓存中，该命令也可以以相同的方式工作。

有关 Redis Lua 脚本的详细信息，请参阅EVAL文档。

## 返回值

[批量字符串回复 ](https://redis.io/topics/protocol#bulk-string-reply)此命令返回添加到脚本缓存中的脚本的SHA1摘要。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18