# script exists

```javascript
SCRIPT EXISTS sha1 [sha1 ...]
```

**自2.6.0起可用。**

**时间复杂度：** O（N），其中N是要检查的脚本的数量（因此检查单个脚本是O（1）操作）。

返回脚本缓存中有关脚本存在的信息。

该命令接受一个或多个 SHA1 摘要，并返回一个1或0的列表，以指示脚本是否已经定义或不在脚本缓存中。在流水线操作之前，这可以确保加载脚本（如果不加载，则使用 SCRIPT LOAD 加载它们），以便可以仅使用 EVALSHA 而不是 EVAL 来执行流水线操作以节省带宽。

有关 Redis Lua 脚本的详细信息，请参阅 EVAL 文档。

## 返回值

[Array reply](https://redis.io/topics/protocol#array-reply) 命令返回与指定的 SHA1 摘要参数对应的整数数组。对于脚本缓存中实际存在的脚本的每个对应 SHA1 摘要，返回1，否则返回0。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18