# script kill

```javascript
SCRIPT KILL
```

**自2.6.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

如果脚本尚未执行写操作，则杀死当前正在执行的 Lua 脚本。

此命令主要用于杀死运行时间过长的脚本（例如，因为它由于错误而进入无限循环）。该脚本将被终止，并且当前阻止进入 EVAL 的客户端将看到该命令返回错误。

如果脚本已经执行了写操作，则它不能以这种方式被杀死，因为它会违反 Lua 脚本原子性合约。在这种情况下，只能`SHUTDOWN NOSAVE`杀死脚本，严重破坏 Redis 进程，阻止它以半写的信息持续存在。

有关 Redis Lua 脚本的详细信息，请参阅EVAL文档。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18