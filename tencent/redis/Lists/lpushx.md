# lpushx

```javascript
LPUSHX key value
```

**自2.2.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

只有在已经存在并且拥有列表的情况`value`下`key`，插入存储在列表的头部`key`。与LPUSH相反，`key`存在时不执行任何操作。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：推送操作后列表的长度。

## 例子

redis> LPUSH mylist "World" `(integer) 1` redis> LPUSHX mylist "Hello" `(integer) 2` redis> LPUSHX myotherlist "Hello" `(integer) 0` redis> LRANGE mylist 0 -1 `1) "Hello" 2) "World"` redis> LRANGE myotherlist 0 -1 `(empty list or set)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18