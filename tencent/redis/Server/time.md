# time

```javascript
TIME
```

**自2.6.0起可用。**

**时间复杂度：** O（1）

TIME 命令以两项列表的形式返回当前服务器时间：Unix 时间戳和当前秒已经过去的微秒数。基本上接口与`gettimeofday`系统调用非常相似。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)，具体为：

包含两个元素的多批量回复：

- unix 时间以秒为单位。

- 微秒。

## 例子

redis> TIME `1) "1506272591" 2) "733053"` redis> TIME `1) "1506272591" 2) "734615"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18