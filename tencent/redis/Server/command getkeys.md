# command getkeys

```javascript
COMMAND GETKEYS
```

**自2.8.13起可用。**

[纠错](javascript:;)

**时间复杂度：** O（N）其中 N 是命令参数的数量

从完整的 Redis 命令返回密钥的 [Array 回复](https://redis.io/topics/protocol#array-reply)。

命令 GETKEYS 是帮助程序命令，可让您从完整的 Redis 命令中找到密钥。

COMMAND 将一些命令显示为具有可移动键，这意味着必须解析整个命令以发现存储或检索关键字。您可以使用 COMMAND GETKEYS 直接从 Redis 分析命令的方式发现关键位置。

## 返回值

[阵列答复](https://redis.io/topics/protocol#array-reply)：您的命令中的键列表。

## 例子

redis> COMMAND GETKEYS MSET a b c d e f `1) "a" 2) "c" 3) "e"` redis> COMMAND GETKEYS EVAL "not consulted" 3 key1 key2 key3 arg1 arg2 arg3 argN `1) "key1" 2) "key2" 3) "key3"` redis> COMMAND GETKEYS SORT mylist ALPHA STORE outlist `1) "mylist" 2) "outlist"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18