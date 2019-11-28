# msetnx

```javascript
MSETNX key value [key value ...]
```

**自1.0.1起可用。**

**时间复杂度：** O（N）其中N是要设置的键的数量。

将给定的键设置为它们各自的值。即使只有一个密钥已经存在，MSETNX 也不会执行任何操作。

由于这种语义，可以使用 MSETNX 来设置代表唯一逻辑对象的不同字段的不同键，以确保所有字段或根本不设置所有字段。

MSETNXT是原子的，所以所有给定的键都被设置一次。客户不可能看到某些密钥已更新，而其他密钥未更改。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果所有的键都设置好了。

- `0` 如果没有设置密钥（至少有一个密钥已经存在）。

## 例子

redis> MSETNX key1 "Hello" key2 "there" `(integer) 1` redis> MSETNX key2 "there" key3 "world" `(integer) 0` redis> MGET key1 key2 key3 `1) "Hello" 2) "there" 3) (nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18