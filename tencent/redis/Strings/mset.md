# mset

```javascript
MSET key value [key value ...]
```

**自1.0.1起可用。**

**时间复杂度：** O（N）其中N是要设置的键的数量。

将给定的键设置为它们各自的值。与常规 SET 一样，MSET 用新值替换现有值。如果您不想覆盖现有值，请参阅 MSETNX。

MSET 是原子的，所以所有给定的键都被设置一次。客户不可能看到某些密钥已更新，而其他密钥未更改。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：总是`OK`因为 MSET 不能失败。

## 例子

redis> MSET key1 "Hello" key2 "World" `"OK"` redis> GET key1 `"Hello"` redis> GET key2 `"World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18