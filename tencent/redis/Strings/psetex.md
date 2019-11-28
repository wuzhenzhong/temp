# psetex

```javascript
PSETEX key milliseconds value
```

**自2.6.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

PSETEX 的工作方式与 SETEX 完全相同，唯一的区别是过期时间以毫秒而不是秒来指定。

## 例子

redis> PSETEX mykey 1000 "Hello" `"OK"` redis> PTTL mykey `(integer) 996` redis> GET mykey `"Hello"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18