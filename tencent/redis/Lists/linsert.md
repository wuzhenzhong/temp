# linsert

```javascript
LINSERT key BEFORE|AFTER pivot value
```

**自2.2.0起可用。**

**时间复杂度：** O（N）其中N是在查看数据透视之前要遍历的元素的数量。这意味着插入列表（头部）左端的某处可以被认为是O（1），并且在右端（尾部）的某处插入的是O（N）。

插入`value`存储在`key`参考值之前或之后的列表中`pivot`。

当`key`不存在时，它被视为空列表，不执行任何操作。

`key`存在但不包含列表值时会返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：插入操作后列表的长度，或者未找到`-1`值`pivot`。

## 例子

redis> RPUSH mylist "Hello" `(integer) 1` redis> RPUSH mylist "World" `(integer) 2` redis> LINSERT mylist BEFORE "World" "There" `(integer) 3` redis> LRANGE mylist 0 -1 `1) "Hello" 2) "There" 3) "World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18