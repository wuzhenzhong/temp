# lrange

```javascript
LRANGE key start stop
```

**自1.0.0起可用。**

**时间复杂度：** O（S + N）其中S是小列表从HEAD开始偏移的距离，大列表从最近端（HEAD或TAIL）开始偏移距离; N是指定范围内的元素数量。

返回存储在列表中的指定元素`key`。偏移`start`和`stop`是基于零的索引，与`0`作为列表（该列表的头部）的第一个元素，`1`成为下一个元件等。

这些偏移量也可以是表示从列表末尾开始的偏移量的负数。例如，`-1`是列表的最后一个元素，`-2`倒数第二个元素，等等。

## 与各种编程语言的范围函数保持一致

请注意，如果您有一个从0到100的数字列表，`LRANGE list 0 10`将返回11个元素，即包含最右边的项目。这**可能会或可能不会**与在您选择的编程语言范围相关的功能（认为Ruby的行为是一致的`Range.new`，`Array#slice`或Python的`range()`功能）。

## 超出范围的索引

超出范围的索引不会产生错误。如果`start`大于列表的末尾，则返回空列表。如果`stop`大于列表的实际末尾，则Redis会将其视为列表的最后一个元素。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：指定范围内的元素列表。

## 例子

redis> RPUSH mylist "one" `(integer) 1` redis> RPUSH mylist "two" `(integer) 2` redis> RPUSH mylist "three" `(integer) 3` redis> LRANGE mylist 0 0 `1) "one"` redis> LRANGE mylist -3 2 `1) "one" 2) "two" 3) "three"` redis> LRANGE mylist -100 100 `1) "one" 2) "two" 3) "three"` redis> LRANGE mylist 5 10 `(empty list or set)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18