# setbit

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)：`OK`如果 SET 正确执行。[空回复](https://redis.io/topics/protocol#nil-reply)：如果由于用户指定了`NX`or `XX`选项但未满足条件而未执行 SET 操作，则返回 Null Bulk Reply 。

## 例子

redis> SET mykey "Hello" `"OK"` redis> GET mykey `"Hello"`

## 模式

**注意：**以下模式不推荐[使用Redlock算法](http://redis.io/topics/distlock)，[该算法](http://redis.io/topics/distlock)实现起来稍微复杂一些，但提供了更好的保证并具有容错性。

该命令`SET resource-name anystring NX EX max-lock-time`是使用Redis实现锁定系统的简单方法。

如果上述命令返回`OK`（或者如果命令返回 Nil 后一段时间后重试），则客户端可以获取锁，并使用 DEL 删除锁。

锁定将在到期时间后自动释放。

可以使这个系统更健壮地修改解锁模式，如下所示：

- 设置一个不可猜测的大型随机字符串（称为标记），而不是设置固定字符串。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18