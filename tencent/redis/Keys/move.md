# move

```javascript
MOVE key db
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

移动`key`从当前选择的数据库（参见 SELECT）到指定的目标数据库。当`key`已经存在于目标数据库中，或者它不存在于源数据库中时，它什么都不做。因此，可以将 MOVE 用作锁定原语。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1`如果`key`被移动。

- `0`如果`key`没有移动。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  