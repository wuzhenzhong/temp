# getbit

```javascript
GETBIT key offset
```

**自2.2.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

返回存储在*键*中的字符串值中*偏移量*的位值。

当*偏移量*超出字符串长度时，字符串被假定为0位的连续空间。当*密钥*不存在时，它被假定为空字符串，所以*偏移*总是超出范围，并且该值也被假定为具有0位的连续空间。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：存储在*偏移*处的位值。

## 例子

redis> SETBIT mykey 7 1 `(integer) 0` redis> GETBIT mykey 0 `(integer) 0` redis> GETBIT mykey 7 `(integer) 1` redis> GETBIT mykey 100 `(integer) 0`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18