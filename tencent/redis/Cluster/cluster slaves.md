# cluster slaves

```javascript
CLUSTER SLAVES node-id
```

**自3.0.0起可用。**

**时间复杂度：** O（1）

该命令提供从指定主节点复制的从节点列表。列表以 CLUSTER NODES 使用的相同格式提供（请参阅其文档以了解格式的规范）。

根据接收命令的节点的节点表，如果指定节点未知或者它不是主节点，则该命令将失败。

请注意，如果从属节点被添加，移动或从给定的主节点中移除，并且我们要求 CLUSTER SLAVES 节点还没有收到配置更新，它可能会显示过时的信息。然而，最终（如果没有网络分区，只需几秒钟），所有的节点都会同意与给定主设备相关的一组节点。

## 返回值

该命令以与 CLUSTER NODES 相同的格式返回数据。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18