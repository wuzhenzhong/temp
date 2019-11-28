# lrem

```javascript
LREM key count value
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是列表的长度。

从存储在列表中`count`的元素中删除第一次出现的元素。这个论点以如下方式影响着操作：`valuekeycount`

- `count > 0`：删除相当于`value`从头到尾移动的元素。

- `count < 0`：删除等于`value`从尾部移动到头部的元素。

- `count = 0`：删除所有等于的元素`value`。

例如，`LREM list -2 "hello"`将删除`"hello"`存储在列表中的最后两个匹配项`list`。

请注意，不存在的键被视为空列表，所以当`key`不存在时，该命令将始终返回`0`。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：删除的元素数量。

## 例子

redis> RPUSH mylist "hello" `(integer) 1` redis> RPUSH mylist "hello" `(integer) 2` redis> RPUSH mylist "foo" `(integer) 3` redis> RPUSH mylist "hello" `(integer) 4` redis> LREM mylist -2 "hello" `(integer) 2` redis> LRANGE mylist 0 -1 `1) "hello" 2) "foo"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18