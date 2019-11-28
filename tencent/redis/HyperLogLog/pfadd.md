# pfadd

```javascript
PFADD key element [element ...]
```

**自2.8.9起可用。**

**时间复杂度：** O（1）添加每个元素。

将所有元素参数添加到以指定为第一个参数的变量名称存储的HyperLogLog数据结构中。

作为该命令的副作用，HyperLogLog 内部件可能会更新以反映迄今为止添加的唯一项目数量（集合的基数）的不同估计值。

如果 HyperLogLog 估计的近似基数在执行命令后发生改变，则PFADD 返回1，否则返回0。如果指定的键不存在，该命令将自动创建一个空的 HyperLogLog 结构（即，具有指定长度和给定编码的Redis字符串）。

要调用没有元素的命令，但只是变量名是有效的，如果变量已经存在，这将导致不执行任何操作，或者如果该键不存在，则只是创建数据结构（在后一种情况下返回1） 。

有关 HyperLogLog 数据结构的介绍，请查看 PFCOUNT 命令页面。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- 1如果至少有1个HyperLogLog内部寄存器被更改。否则为0。

## 例子

redis> PFADD hll a b c d e f g `(integer) 1` redis> PFCOUNT hll `(integer) 7`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18