# scard

```javascript
SCARD key
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

返回存储的集合的集合基数（元素数量）`key`。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：集合的基数（元素数量），或者`0`如果`key`不存在。

## 例子

redis> SADD myset "Hello" `(integer) 1` redis> SADD myset "World" `(integer) 1` redis> SCARD myset `(integer) 2`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18