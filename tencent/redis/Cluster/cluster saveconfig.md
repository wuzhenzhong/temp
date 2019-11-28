# cluster saveconfig

```javascript
CLUSTER SAVECONFIG
```

**自3.0.0起可用。**

**时间复杂度：** O（1）

强制节点将`nodes.conf`配置保存到磁盘上。在返回命令调用`fsync(2)`之前，为了确保配置在计算机磁盘上被刷新。

[纠错](javascript:;)

该命令主要用于`nodes.conf`节点状态文件由于某种原因丢失/删除的情况，我们希望从头开始再次生成它。它也可以用于通过`CLUSTER`命令对节点群集配置进行普通改动，以确保新配置在磁盘上保持不变，但是所有命令通常应该能够自动调度，以便在磁盘上保存配置时重要的是为了在重新启动时系统的正确性。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果操作失败，则返回错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18