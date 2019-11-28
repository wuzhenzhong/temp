# smembers

```javascript
SMEMBERS key
```

**自1.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N）其中N是集合基数。

返回所存储的设置值的所有成员`key`。

这与使用一个参数运行 SINTER 具有相同的效果`key`。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：集合中的所有元素。

## 例子

redis> SADD myset "Hello" `(integer) 1` redis> SADD myset "World" `(integer) 1` redis> SMEMBERS myset `1) "Hello" 2) "World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18