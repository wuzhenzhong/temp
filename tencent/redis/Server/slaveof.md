# slaveof

```javascript
SLAVEOF host port
```

**自1.0.0起可用。**

SLAVEOF 命令可以即时更改从站的复制设置。如果 Redis 服务器已充当从服务器，则命令 SLAVEOF NO ONE 将关闭复制，将 Redis 服务器变为主服务器。以适当的形式，SLAVEOF 主机名端口将使服务器成为监听指定主机名和端口的另一台服务器的从服务器。

如果服务器已经是某个主服务器的从服务器，则 SLAVEOF 主机名端口将停止针对旧服务器的复制，并针对新服务器启动同步，丢弃旧数据集。

表单 SLAVEOF NO ONE 将停止复制，将服务器变为 MASTER ，但不会放弃复制。因此，如果旧主设备停止工作，可以将从设备变为主设备，并设置应用程序以读/写方式使用这个新主设备。稍后当其他 Redis 服务器被修复时，它可以重新配置为从服务器。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

**关于从机的说明**：不幸的是，最初的主从术语是为数据库选择的。当设计 Redis 时，现有的术语没有太多的选择分析，但是 **SLAVEOF NO ONE** 命令被添加为自由信息。我们不想改变那些需要打破 API 和 INFO 输出的向后兼容性的术语，而是想用这个页面提醒你，从机既是**对人类的犯罪，也是对人类**[历史](https://en.wikipedia.org/wiki/Slavery)持续存在的东西。

*如果奴隶制没有错，没有什么是错的。* - 亚伯拉罕·林肯

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18