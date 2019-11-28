# ltrim

```javascript
LTRIM key start stop
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是操作要删除的元素的数量。

修剪现有列表，使其仅包含指定的指定范围的元素。两个`start`和`stop`是基于零的索引，其中，`0`是列表（头），的第一个元素`1`的下一个元素等。

例如：`LTRIM foobar 0 2`将修改存储在列表中的列表，`foobar`以便只保留列表的前三个元素。

`start`和`end`也可以是指示从列表末尾偏移的负数，列表`-1`的最后一个元素，`-2`倒数第二个元素等等。

超出范围的索引不会产生错误：如果`start`大于列表的末尾，或者`start > end`结果将是空列表（导致`key`被删除）。如果`end`大于列表的末尾，则Redis会将其视为列表的最后一个元素。

LTRIM的常见用法是与LPUSH / RPUSH一起使用。例如：

```javascript
LPUSH mylist someelement
LTRIM mylist 0 99
```

这一对命令将推送列表中的一个新元素，同时确保该列表的长度不会超过100个元素。例如，使用Redis存储日志时，这非常有用。需要注意的是，当以这种方式使用LTRIM时，OTRIM是O（1）操作，因为在平均情况下，只有一个元素从列表的尾部被删除。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> RPUSH mylist "one" `(integer) 1` redis> RPUSH mylist "two" `(integer) 2` redis> RPUSH mylist "three" `(integer) 3` redis> LTRIM mylist 1 -1 `"OK"` redis> LRANGE mylist 0 -1 `1) "two" 2) "three"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18