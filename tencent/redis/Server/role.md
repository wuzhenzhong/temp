# role

```javascript
ROLE
```

**自2.8.12起可用。**

提供有关 Redis 实例在复制上下文中的角色的信息，如果实例当前是，或`master`，则返回。该命令还返回有关复制状态（如果角色是主控或从属角色）或监控主控名称列表（如果角色是标志）的其他信息。`slavesentinel`

## 输出格式

该命令返回一组元素。第一个元素是实例的角色，作为以下三个字符串之一：

- "master"

- "slave"

- "sentinel"

数组的其他元素取决于角色。

## 主输出

在主实例中调用 ROLE 时的输出示例：

```javascript
1) "master"
2) (integer) 3129659
3) 1) 1) "127.0.0.1"
      2) "9001"
      3) "3129242"
   2) 1) "127.0.0.1"
      2) "9002"
      3) "3129543"
```

主输出由以下部分组成：

\1. 字符串`master`。

\2. 当前的主复制偏移量是主控和从属共享的偏移量，在部分重新同步中，副控制器需要提取的部分才能继续。

\3. 由三个元素组成的数组表示连接的从站。每个子阵列都包含从站IP，端口和最后一个确认的复制偏移量。

## 从机输出

在从属实例中调用 ROLE 时的输出示例：

```javascript
1) "slave"
2) "127.0.0.1"
3) (integer) 9000
4) "connected"
5) (integer) 3167038
```

从站输出由以下部分组成：

\1. 字符串`slave`。

\2. 主设备的IP。

\3. 主设备的端口号。

\4. 从主设备的角度来看，复制的状态可以是`connect`（实例需要连接到它的主设备），`connecting`（从设备 - 主设备连接正在进行），`sync`（主设备和从设备正在尝试执行同步），`connected`（奴隶在线）。

\5. 根据主复制偏移量，从从站接收的数据量。

## 哨兵输出

Sentinel 输出示例：

```javascript
1) "sentinel"
2) 1) "resque-master"
   2) "html-fragments-master"
   3) "stats-master"
   4) "metadata-master"
```

哨兵输出由以下部分组成：

\1. 字符串`sentinel`。

\2. 由此 Sentinel 实例监控的主名称数组。

## 返回值

[阵列答复](https://redis.io/topics/protocol#array-reply)：其中所述第一元件是一个`master`，`slave`，`sentinel`和所述附加元件是如上示出的特定角色。

## History

- 该命令是在 Redis 稳定版本中引入的，特别是在 Redis 2.8.12中。

## 例子

redis> ROLE `1) "master" 2) (integer) 0 3) (empty list or set)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18