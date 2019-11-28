# touch

```javascript
TOUCH key [key ...]
```

**自3.2.1起可用。**

**时间复杂度：** O（N）其中N是将被触摸的键的数量。

更改密钥的最后访问时间。如果一个密钥不存在，它将被忽略。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)：被触摸的键的数量。

## 例子

redis> SET key1 "Hello" `"OK"` redis> SET key2 "World" `"OK"` redis> TOUCH key1 key2 `ERR Unknown or disabled command 'TOUCH'`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  