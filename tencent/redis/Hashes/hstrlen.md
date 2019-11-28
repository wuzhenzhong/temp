# hstrlen

```javascript
HSTRLEN key field
```

**自3.2.0起可用。**

**时间复杂度：** O（1）

[纠错](javascript:;)

返回与相关联的值的字符串长度`field`在存储在散列`key`。如果`key`或者`field`不存在，则返回0。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：与哈希关联的值的字符串长度`field`，或者当`field`哈希`key`中不存在或根本不存在时为零。

## 例子

redis> HMSET myhash f1 HelloWorld f2 99 f3 -256 `"OK"` redis> HSTRLEN myhash f1 `(integer) 10` redis> HSTRLEN myhash f2 `(integer) 2` redis> HSTRLEN myhash f3 `(integer) 4`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18