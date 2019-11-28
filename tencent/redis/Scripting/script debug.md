# script debug

```javascript
SCRIPT DEBUG YES|SYNC|NO
```

**自3.2.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

为 EVAL 执行的后续脚本设置调试模式。Redis 包含一个完整的 Lua 调试器，代号为 LDB，可用于编写复杂脚本的任务变得更加简单。在调试模式下，Redis 充当远程调试服务器，客户端`redis-cli`可以一步一步地执行脚本，设置断点，检查变量等 - 有关 LDB 的更多信息，请参阅 [Redis Lua调试器 ](https://redis.io/topics/ldb)页面。

**重要说明：**避免使用 Redis 生产服务器调试 Lua 脚本。改用开发服务器。

可以使用两种模式之一来启用 LDB：异步或同步。在异步模式下，服务器创建一个分叉的调试会话，该会话不会阻塞，并且在会话结束后**回滚**数据的所有更改，因此可以使用相同的初始状态重新启动调试。备用同步调试模式会在调试会话处于活动状态时阻塞服务器，并在数据集结束后保留对数据集的所有更改。

- `YES`。启用 Lua 脚本的非阻塞异步调试（更改将被丢弃）。

- 同步。启用阻止 Lua 脚本的同步调试（保存对数据的更改）。

- `NO`。禁用脚本调试模式。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18