# pfmerge

```javascript
PFMERGE destkey sourcekey [sourcekey ...]
```

**自2.8.9起可用。**

**时间复杂度：** O（N）合并N个 HyperLogLog，但具有较高的常量时间。

将多个 HyperLogLog 值合并为一个唯一值，该值将近似观察到的源 HyperLogLog 结构集的联合的基数。

计算出的合并 HyperLogLog 被设置为目标变量，如果不存在，则创建目标变量（默认为空的 HyperLogLog）。

## 返回值

[简单的字符串回复](https://redis.io/topics/protocol#simple-string-reply)：该命令刚刚返回`OK`。

## 例子

redis> PFADD hll1 foo bar zap a `(integer) 1` redis> PFADD hll2 a b c foo `(integer) 1` redis> PFMERGE hll3 hll1 hll2 `"OK"` redis> PFCOUNT hll3 `(integer) 6`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18