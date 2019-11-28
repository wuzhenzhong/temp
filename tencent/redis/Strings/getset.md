# getset

```javascript
GETSET key value
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

以原子方式设置`key`到`value`，并返回存储在旧值`key`。`key`存在但返回一个错误，但不包含字符串值。

## 设计模式

GETSET 可以与 INCR 一起用于计数原子重置。例如：`mycounter`每次某个事件发生时，一个进程可能会调用 INCR 来对付该键，但是我们需要不时地获取该计数器的值并将其重置为零。这可以使用`GETSET mycounter "0"`：

redis> INCR mycounter `(integer) 1` redis> GETSET mycounter "0" `"1"` redis> GET mycounter `"0"`

## 返回值

[散装字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：旧值存储在`key`，或者`nil`当`key`不存在。

## 例子

redis> SET mykey "Hello" `"OK"` redis> GETSET mykey "World" `"Hello"` redis> GET mykey `"World"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18