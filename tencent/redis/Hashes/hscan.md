# hscan

```javascript
HSCAN key cursor [MATCH pattern] [COUNT count]
```

**自2.8.0起可用。**

**时间复杂度：** O（1）用于每个呼叫。O（N）进行完整的迭代，包括足够的命令调用以使游标返回0。 N 是集合中元素的数量。

[纠错](javascript:;)

有关 HSCAN 文档，请参阅 SCAN 。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18