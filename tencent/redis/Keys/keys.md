# keys

```javascript
KEYS pattern
```

**自1.0.0起可用。**

**时间复杂度：** O（N），其中N是数据库中的键的数量，假设数据库中的键名和给定模式的长度有限。

返回所有匹配的键`pattern`。

虽然此操作的时间复杂度为O（N），但常数时间相当短。例如，运行在入门级笔记本电脑上的 Redis 可以在40毫秒内扫描一百万个密钥数据库。

**警告**：将 KEYS 视为仅应在生产环境中谨慎使用的命令。在对大型数据库执行它时可能会损坏性能。此命令用于调试和特殊操作，例如更改您的密钥空间布局。请勿在常规应用程序代码中使用KEYS。如果您正在寻找一种方法在您的密钥空间的子集中查找密钥，请考虑使用 SCAN 或[集合](https://redis.io/topics/data-types#sets)。

支持的全局样式模式：

- `h?llo` matches `hello`, `hallo` and `hxllo`

- `h*llo` matches `hllo` and `heeeello`

- `h[ae]llo` matches `hello` and `hallo,` but not `hillo`

- `h[^e]llo` matches `hallo`, `hbllo`, ... but not `hello`

- `h[a-b]llo` matches `hallo` and `hbllo`

`\`如果您想逐字匹配，请使用转义特殊字符。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)：密钥匹配列表`pattern`。

## 例子

redis> MSET one 1 two 2 three 3 four 4 `"OK"` redis> KEYS *o* `1) "one" 2) "four" 3) "two"` redis> KEYS t?? `1) "two"` redis> KEYS * `1) "three" 2) "one" 3) "four" 4) "two"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  