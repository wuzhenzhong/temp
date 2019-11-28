# rpop

```javascript
RPOP key
```

**自1.0.0起可用。**

**Time complexity:** O(1)

删除并返回存储在列表中的最后一个元素`key`。

## 返回值

[散装串答复](https://redis.io/topics/protocol#bulk-string-reply)：最后一个元素的值，返回`nil`当`key`不存在。

## 例子

redis> RPUSH mylist "one" `(integer) 1` redis> RPUSH mylist "two" `(integer) 2` redis> RPUSH mylist "three" `(integer) 3` redis> RPOP mylist `"three"` redis> LRANGE mylist 0 -1 `1) "one" 2) "two"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18