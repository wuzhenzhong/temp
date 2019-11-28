# sinter

```javascript
SINTER key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N * M）最差情况，其中N是最小集合的基数，M是集合的数量。

返回由所有给定集合的交集所产生的集合的成员。

例如：

```javascript
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SINTER key1 key2 key3 = {c}
```

不存在的键被认为是空集。其中一个键是一个空集，结果集也是空的（因为与空集的交集总是导致一个空集）。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：列出结果集的成员。

## 例子

redis> SADD key1 "a" `(integer) 1` redis> SADD key1 "b" `(integer) 1` redis> SADD key1 "c" `(integer) 1` redis> SADD key2 "c" `(integer) 1` redis> SADD key2 "d" `(integer) 1` redis> SADD key2 "e" `(integer) 1` redis> SINTER key1 key2 `1) "c"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18