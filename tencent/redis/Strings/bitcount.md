# bitcount

```javascript
BITCOUNT key [start end]
```

**自2.6.0起可用。**

**时间复杂度：** O（N）

[纠错](javascript:;)

计算字符串中的设置位数（人口计数）。

默认情况下，会检查字符串中包含的所有字节。只能在传递附加参数 *start* 和 *end* 的间隔中指定计数操作。

与 GETRANGE 命令类似，开始和结束可以包含负值，以便从字符串的末尾开始索引字节，其中-1是最后一个字节，-2是倒数第二个字符，等等。

不存在的键被视为空字符串，因此该命令将返回零。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)

位数设置为1。

## 例子

redis> SET mykey "foobar" `"OK"` redis> BITCOUNT mykey `(integer) 26` redis> BITCOUNT mykey 0 0 `(integer) 4` redis> BITCOUNT mykey 1 1 `(integer) 6`

## 模式：使用位图的实时指标

位图是某些类型信息的非常节省空间的表示。一个例子是需要用户访问历史记录的 Web 应用程序，例如，可以确定哪些用户是测试版功能的良好目标。

使用 SETBIT 命令可以很轻松地完成，每天用一个小渐进整数标识。例如，第0天是应用程序上线的第一天，第二天的第1天等等。

每次用户执行页面查看时，应用程序都可以在当天使用 SETBIT 命令访问网站，并设置当天对应的位。

稍后，知道用户访问网站的单天数量简单地调用 BITCOUNT 命令对位图将是微不足道的。

在名为“ [使用Redis位图的快速简单实时指标](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps) ”的文章中介绍了使用用户标识代替天数的类似模式。

## 性能考虑

在上述计算日期的示例中，即使10年后应用程序处于联机状态，我们仍然只有`365*10`每位用户的数据位，即每位用户只有456个字节。有了这个数据量，BITCOUNT 仍然像任何其他O（1）Redis命令一样快，如 GET 或 INCR 。

当位图很大时，有两种选择：

- 采取每次修改位图时分离的密钥。使用小型 Redis Lua 脚本，这可以非常高效并且原子化。

- 使用 BITCOUNT *开始*和*结束*可选参数递增地运行位图，累积客户端的结果，并可选择将结果缓存到密钥中。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18