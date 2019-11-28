# persist

```javascript
PERSIST key
```

**自2.2.0起可用。**

**时间复杂度：** O（1）

[纠错](javascript:;)

删除现有的超时值`key`，将密钥从*易失性*（具有过期集的密钥）转换为*持久性*（由于没有超时关联，密钥永不过期）。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果超时被删除。

- `0`如果`key`不存在或没有关联的超时。

## 例子

redis> SET mykey "Hello" `"OK"` redis> EXPIRE mykey 10 `(integer) 1` redis> TTL mykey `(integer) 10` redis> PERSIST mykey `(integer) 1` redis> TTL mykey `(integer) -1`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  