# pubsub

```javascript
PUBSUB subcommand [argument [argument ...]]
```

**自2.8.0起可用。**

**时间复杂度：**对于CHANNELS子命令的O（N），其中N是活动通道的数量，并假定恒定时间模式匹配（相对较短的通道和模式）。O（N）表示 NUMSUB 子命令，其中N是请求的通道数。O（1）用于 NUMPAT 子命令。

PUBSUB 命令是一个内省命令，允许检查发布/订阅子系统的状态。它由分开记录的子命令组成。一般形式是：

```javascript
PUBSUB <subcommand> ... args ...
```

列出当前*活动的频道*。活动频道是包含一个或多个订阅者（不包括客户订阅模式）的发布/订阅频道。

如果没有指定 `pattern`，则列出所有通道，否则如果指定了pattern，则只列出与指定的全局样式匹配的通道。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：一个活动频道列表，可选地匹配指定的模式。

返回指定通道的订阅者数量（不包括订阅了模式的客户端）。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：每个频道的频道列表和用户数量。格式是频道，计数，频道，计数，...，所以列表是平的。列出通道的顺序与命令调用中指定的通道顺序相同。

请注意，在没有通道的情况下调用此命令是有效的。在这种情况下，它将只返回一个空列表。

返回模式的订阅数（使用 PSUBSCRIBE 命令执行）。请注意，这不仅仅是订阅模式的客户数量，而是所有客户订阅的模式总数。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：所有客户端订阅的模式数量。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18