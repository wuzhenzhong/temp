# sunion

```javascript
SUNION key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是所有给定集合中元素的总数。

[纠错](javascript:;)

返回从所有给定集合的联合中产生的集合的成员。

例如：

```javascript
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SUNION key1 key2 key3 = {a,b,c,d,e}
```

不存在的键被认为是空集。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：列出结果集的成员。

## 例子

redis> SADD key1 "a" `(integer) 1` redis> SADD key1 "b" `(integer) 1` redis> SADD key1 "c" `(integer) 1` redis> SADD key2 "c" `(integer) 1` redis> SADD key2 "d" `(integer) 1` redis> SADD key2 "e" `(integer) 1` redis> SUNION key1 key2 `1) "a" 2) "c" 3) "e" 4) "b" 5) "d"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18