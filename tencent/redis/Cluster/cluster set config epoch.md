# cluster set config epoch

```javascript
CLUSTER SET-CONFIG-EPOCH config-epoch
```

**自3.0.0起可用。**

**时间复杂度：** O（1）

该命令在新节点中设置特定的*配置时期*。它只适用于以下情况：

[纠错](javascript:;)

\1. 节点的节点表是空的。

\2. 节点当前*配置时期*为零。

这些先决条件是必需的，因为通常情况下，手动更改节点的配置时期是不安全的，我们希望确保具有较高配置时期值（即最后一次故障切换）的节点胜过其他节点声称散列槽所有权。

但是，此规则有一个例外，并且是从头开始创建新群集的时候。Redis 集群*配置时期冲突解决*算法可以在启动时处理所有使用相同配置配置的新节点，但是此过程很慢并且应该是例外，只是为了确保无论发生什么情况，两个以上的节点最终总是远离状态具有相同的配置时期。

因此，使用`CONFIG SET-CONFIG-EPOCH`，在创建新群集时，我们可以在将群集加入到一起之前为每个节点分配一个不同的渐进配置历元。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果命令执行成功，否则返回错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18