# cluster info

```javascript
CLUSTER INFO
```

**自3.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

CLUSTER INFO 提供有关 Redis Cluster 重要参数的 INFO 样式信息。以下是一个输出示例，接下来是报告的每个字段的描述。

```javascript
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:2
cluster_stats_messages_sent:1483972
cluster_stats_messages_received:1483968
```

- `cluster_state`：状态是`ok`节点是否能够接收查询。`fail`如果至少有一个未绑定的散列槽（没有关联的节点），处于错误状态（为其服务的节点被标记为 FAIL 标记），或者该节点无法到达大多数主节点。

- `cluster_slots_assigned`：与某个节点关联的槽数（不是未绑定的）。这个数字应该是16384，节点才能正常工作，这意味着每个散列槽应该映射到一个节点。

- `cluster_slots_ok`：映射到不处于`FAIL`或`PFAIL`处于状态的节点的散列槽的数量。

- `cluster_slots_pfail`：映射到处于`PFAIL`状态的节点的散列槽的数量。请注意，只要`PFAIL`状态不由`FAIL`故障检测算法提升，这些散列槽仍可正常工作。`PFAIL`仅意味着我们目前无法与节点通话，但可能只是一个暂时的错误。

- `cluster_slots_fail`：映射到处于`FAIL`状态的节点的散列槽的数量。如果此数字不为零，则该节点无法提供查询，除非在配置中`cluster-require-full-coverage`设置为`no`。

- `cluster_known_nodes`：群集中已知节点的总数，包括`HANDSHAKE`当前可能不是群集适当成员的状态节点。

- `cluster_size`：服务群集中至少一个散列槽的主节点的数量。

- `cluster_current_epoch`：局部`Current Epoch`变量。这用于在故障转移期间创建独特的增加版本号。

- `cluster_my_epoch`：我们正在与之交谈的`Config Epoch`节点。这是分配给此节点的当前配置版本。

- `cluster_stats_messages_sent`：通过集群节点到节点二进制总线发送的消息数量。

- `cluster_stats_messages_received`：通过集群节点到节点二进制总线接收的消息数量。

有关“当前时代”和“配置时期”变量的更多信息可在 Redis 集群规范文档中找到。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：指定字段和值之间的映射，其格式`<field>:<value>`由以两个字节组成的换行符分隔`CRLF`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18