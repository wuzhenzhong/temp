# brpop

```javascript
BRPOP key [key ...] timeout
```

**自2.0.0起可用。**

**时间复杂度：** O（1）

BRPOP is a blocking list pop primitive. It is the blocking version of RPOP because it blocks the connection when there are no elements to pop from any of the given lists. An element is popped from the tail of the first list that is non-empty, with the given keys being checked in the order that they are given.

请参阅BLPOP文档以了解确切的语义，因为BRPOP与BLPOP相同，唯一的区别是它从列表的尾部弹出元素，而不是从头部弹出。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：具体为：

- `nil`当没有元件可以被弹出多批量和超时过期。

- 第一个元素是元素被弹出的键的名称，第二个元素是弹出元素的值的两元素的多块。

## 例子

```javascript
redis> DEL list1 list2
(integer) 0
redis> RPUSH list1 a b c
(integer) 3
redis> BRPOP list1 list2 0
1) "list1"
2) "c"
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18