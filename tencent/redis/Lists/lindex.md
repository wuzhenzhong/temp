# lindex

```javascript
LINDEX key index
```

**自1.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N）其中N是要到达索引处元素的遍历元素的数量。这使得询问列表O（1）的第一个或最后一个元素。

返回`index`存储在列表中的索引处的元素`key`。该索引是基于零的，因此`0`意味着第一个元素，`1`第二个元素等等。负数索引可用于指定从列表尾部开始的元素。这里`-1`意味着最后一个元素，`-2`意味着倒数第二个元素等等。

当值 `key`不是列表时，返回错误。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：请求的元素返回`nil`当`index`超出范围。

## 例子

redis> LPUSH mylist "World" `(integer) 1` redis> LPUSH mylist "Hello" `(integer) 2` redis> LINDEX mylist 0 `"Hello"` redis> LINDEX mylist -1 `"World"` redis> LINDEX mylist 3 `(nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18