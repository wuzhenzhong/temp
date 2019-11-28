# rpush

```javascript
RPUSH key value [value ...]
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

将所有指定值插入存储在列表的尾部`key`。如果`key`不存在，则在执行推送操作之前将其创建为空列表。当`key`保存不是列表的值时，将返回错误。

可以使用单个命令调用来推送多个元素，只需在命令末尾指定多个参数即可。元素一个接一个地插入到列表的尾部，从最左边的元素到最右边的元素。因此，例如，该命令`RPUSH mylist a b c`将导致包含第一个元素`a`，第二个元素和`b`第三个元素`c`的列表。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：推送操作后列表的长度。

## 历史

- `>= 2.4`：接受多个`value`参数。在2.4以前的Redis版本中，可以为每个命令推送一个值。

## 例子

redis> RPUSH mylist "hello" `(integer) 1` redis> RPUSH mylist "world" `(integer) 2` redis> LRANGE mylist 0 -1 `1) "hello" 2) "world"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18