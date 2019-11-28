# smove

```javascript
SMOVE source destination member
```

**自1.0.0起可用。**

[纠错](javascript:;)

**时间复杂度：** O（1）

移动`member`在从设置`source`到设定的`destination`。这个操作是原子的。在每一个特定的时刻，元素将显示为成员`source` **或** `destination`其他客户端。

如果源集不存在或不包含指定的元素，则不执行任何操作并`0`返回。否则，该元素将从源集中删除并添加到目标集中。当指定的元素已经存在于目标集中时，它只会从源集中移除。

如果`source`或`destination`没有保持设定值，则返回错误。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果元素被移动。

- `0`如果元素不是成员`source`并且未执行任何操作。

## 例子

redis> SADD myset "one" `(integer) 1` redis> SADD myset "two" `(integer) 1` redis> SADD myotherset "three" `(integer) 1` redis> SMOVE myset myotherset "two" `(integer) 1` redis> SMEMBERS myset `1) "one"` redis> SMEMBERS myotherset `1) "two" 2) "three"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18