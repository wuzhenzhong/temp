# set

```javascript
SET key value [EX seconds] [PX milliseconds] [NX|XX]
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

设置`key`为保存字符串`value`。如果`key`已经保存了一个值，则不管其类型如何，都会被覆盖。在 SET 操作成功之后，任何以前与密钥关联的生存时间都将被丢弃。

## 选项

从 Redis 开始2.6.12 SET 支持一组修改其行为的选项：

- `EX` *秒* - 设置指定的到期时间，以秒为单位。

- `PX` *毫秒* - 设置指定的到期时间，以毫秒为单位。

- `NX` - 只有在密钥不存在的情况下才能设置密钥。

- `XX` - 只有在钥匙已经存在的情况下才能设置。

注意：由于 SET 命令选项可以替代 SETNX，SETEX，PSETEX，因此在未来的 Redis 版本中，这三个命令可能会被弃用并最终被删除。

## 返回值

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

- 不要使用 DEL 来释放锁定，而是发送一个只在值匹配时才删除密钥的脚本。

这样可以避免客户端在过期时间后尝试释放锁，从而删除由稍后获取锁的另一个客户端创建的密钥。

解锁脚本的一个示例与以下内容类似：

```javascript
if redis.call("get",KEYS[1]) == ARGV[1]
then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

该脚本应该用来调用 `EVAL ...script... 1 resource-name token-value`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18