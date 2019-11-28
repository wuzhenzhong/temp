# del

```javascript
DEL key [key ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是将被删除的键的数量。当一个要删除的键持有一个字符串以外的值时，这个键的个体复杂度是O（M），其中M是列表中元素的数量，集合，排序集合或散列值。删除包含字符串值的单个键是O（1）。

删除指定的键。如果一个密钥不存在，它将被忽略。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：已删除的键的数量。

## 例子

redis> SET key1 "Hello" `"OK"` redis> SET key2 "World" `"OK"` redis> DEL key1 key2 key3 `(integer) 2`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18