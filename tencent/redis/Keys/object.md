# object

```javascript
OBJECT subcommand [arguments [arguments ...]]
```

**自2.2.3起可用。**

**时间复杂度：** O（1）适用于所有当前实施的子命令。

OBJECT 命令允许检查与键相关的 Redis 对象的内部。对于调试或了解您的密钥是否使用特殊编码的数据类型以节省空间非常有用。在使用 Redis 作为缓存时，您的应用程序也可能使用 OBJECT 命令报告的信息来实施应用程序级别密钥驱逐策略。

OBJECT命令支持多个子命令：

- `OBJECT REFCOUNT <key>`返回与指定键相关联的值的引用数。该命令主要用于调试。

- `OBJECT ENCODING <key>` 返回用于存储与键关联的值的内部表示形式。

- `OBJECT IDLETIME <key>`返回自指定键存储的对象空闲以来的秒数（未通过读取或写入操作请求）。虽然该值以秒为单位返回，但此计时器的实际分辨率为10秒，但在将来的实施中可能会有所不同。

对象可以用不同的方式进行编码：

- 字符串可以编码为`raw`（普通字符串编码）或`int`（以64位有符号间隔表示整数的字符串以这种方式编码以节省空间）。

- 列表可以编码为`ziplist`或`linkedlist`。本`ziplist`是用来节省空间的小型列出特定表示。

- 集可以编码为`intset`或`hashtable`。这`intset`是一个特殊的编码，用于仅由整数组成的小集合。

- 哈希可以编码为`ziplist`或`hashtable`。这`ziplist`是一个用于小哈希的特殊编码。

- 排序集可以编码为`ziplist`或`skiplist`格式。至于列表类型，小的排序集合可以使用特殊编码`ziplist`，而`skiplist`编码是与任何大小的排序集合一起工作的编码。

所有特殊编码的类型会在您执行操作后自动转换为通用类型，使Redis无法保留节省空间的编码。

## 返回值

不同的子命令使用不同的返回值。

- 子命令`refcount`和`idletime`返回整数。

- 子命令`encoding`返回批量回复。

如果您尝试检查的对象丢失，则返回空批量答复。

## 例子

```javascript
redis> lpush mylist "Hello World"
(integer) 4
redis> object refcount mylist
(integer) 1
redis> object encoding mylist
"ziplist"
redis> object idletime mylist
(integer) 10
```

在以下示例中，您可以看到一旦Redis不再能够使用节省空间的编码，编码如何更改。

```javascript
redis> set foo 1000
OK
redis> object encoding foo
"int"
redis> append foo bar
(integer) 7
redis> get foo
"1000bar"
redis> object encoding foo
"raw"
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  