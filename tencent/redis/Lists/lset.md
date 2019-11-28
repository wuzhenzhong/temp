# lset

```javascript
LSET key index value
```

**自1.0.0起可用。**

**时间复杂度：** O（N）其中N是列表的长度。设置列表的第一个或最后一个元素是O（1）。

将列表元素设置`index`为`value`。有关`index`参数的更多信息，请参阅LINDEX。

超出范围索引会返回错误。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> RPUSH mylist "one" `(integer) 1` redis> RPUSH mylist "two" `(integer) 2` redis> RPUSH mylist "three" `(integer) 3` redis> LSET mylist 0 "four" `"OK"` redis> LSET mylist -2 "five" `"OK"` redis> LRANGE mylist 0 -1 `1) "four" 2) "five" 3) "three"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18