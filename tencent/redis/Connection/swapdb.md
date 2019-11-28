# swapdb

```javascript
SWAPDB index index
```

**自4.0.0起可用。**

[纠错](javascript:;)

该命令交换两个 Redis 数据库，以便立即所有连接到给定数据库的客户端都能看到另一个数据库的数据，反之亦然。例：

```javascript
SWAPDB 0 1
```

这将使数据库0与数据库1交换。与数据库0连接的所有客户端将立即看到新数据，就像连接数据库1的所有客户端都将看到之前数据库0的数据。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply): 如果 SWAPDB 正确执行，则`OK`。

## 例子

redis> SWAPDB 0 1 `ERR Unknown or disabled command 'SWAPDB'`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18