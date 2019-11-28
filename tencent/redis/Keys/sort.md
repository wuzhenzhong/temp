# sort

```javascript
SORT key [BY pattern] [LIMIT offset count] [GET pattern [GET pattern ...]] [ASC|DESC] [ALPHA] [STORE destination]
```

**自1.0.0起可用。**

**时间复杂度：** O（N + M * log（M））其中N是列表中要排序的元素的数量，M是返回元素的数量。当元素未被排序时，复杂度当前为O（N），因为在下一个版本中将会避免复制步骤。

返回或存储包含元素的[列表](https://redis.io/topics/data-types#lists)，[设置](https://redis.io/topics/data-types#set)或[排序集合](https://redis.io/topics/data-types#sorted-sets)的`key`。默认情况下，排序是数字，元素通过将其值解释为双精度浮点数进行比较。这是最简单的SORT形式：

```javascript
SORT mylist
```

假设`mylist`是数字列表，该命令将返回相同的列表，其中的元素从小到大排序。为了将数字从大到小排序，请使用`DESC`修饰符：

```javascript
SORT mylist DESC
```

当`mylist`包含字符串值并且您想按字典顺序对它们进行排序时，请使用`ALPHA`修饰符：

```javascript
SORT mylist ALPHA
```

Redis支持UTF-8，假设您正确设置了`!LC_COLLATE`环境变量。

使用`LIMIT`修饰符可以限制返回元素的数量。该修饰符接受`offset`参数，指定要跳过的元素数和`count`参数，指定从开始处返回的元素数`offset`。以下示例将返回10个排序版本的`mylist`元素，从元素0开始（`offset`从零开始）：

```javascript
SORT mylist LIMIT 0 10
```

几乎所有的修饰符都可以一起使用。以下示例将返回前5个元素，按字典顺序降序排列：

```javascript
SORT mylist LIMIT 0 5 ALPHA DESC
```

## 通过外部键排序

有时候你想使用外部键作为权重进行排序，而不是比较列表，设置或排序集中的实际元素。比方说，清单`mylist`中包含的元素`1`，`2`并`3`表示存储在对象的唯一ID `object_1`，`object_2`和`object_3`。当这些对象都有关联的存储的权重`weight_1`，`weight_2`并且`weight_3`，排序可以指示使用这些权重排序`mylist`用以下语句：

```javascript
SORT mylist BY weight_*
```

该`BY`选项采用`weight_*`用于生成用于排序的键的模式（在本例中相同）。获得这些键名称代的第一次出现`*`在列表中的元件的实际值（`1`，`2`并且`3`在这个例子中）。

## 跳过排序元素

该`BY`选项也可以采用不存在的键，这会导致 SORT 跳过排序操作。如果您想检索外部密钥（请参阅`GET`下面的选项），而没有排序开销，这很有用。

```javascript
SORT mylist BY nosort
```

## 检索外部密钥

我们前面的例子只返回排序后的 ID。在某些情况下，以获得实际的对象，而不是它们的 ID（更多有用的`object_1`，`object_2`和`object_3`）。根据列表中的元素检索外键，可以使用以下命令完成 set 或 sorted set：

```javascript
SORT mylist BY weight_* GET object_*
```

该`GET`选项可以多次使用，以便为原始列表，集合或排序集合的每个元素获取更多的键。

`GET`元素本身也可以使用特殊模式`#`：

```javascript
SORT mylist BY weight_* GET object_* GET #
```

## 存储SORT操作的结果

默认情况下，SORT 将排序后的元素返回给客户端。使用该`STORE`选项，结果将作为列表存储在指定的密钥中，而不是返回给客户端。

```javascript
SORT mylist BY weight_* STORE resultkey
```

使用一个有趣的模式`SORT ... STORE`在于将 EXPIRE 超时与结果键相关联，以便在可以缓存 SORT 操作结果一段时间的应用程序中使用。其他客户端将使用缓存列表，而不是为每个请求调用 SORT。当密钥超时时，可以通过`SORT ... STORE`再次调用来创建缓存的更新版本。

请注意，要正确实现此模式，避免多个客户端同时重建缓存很重要。这里需要某种锁定（例如使用 SETNX）。

## Using hashes in `BY` and `GET`

使用以下语法可以使用`BY`和`GET`选择哈希字段：

```javascript
SORT mylist BY weight_*->fieldname GET object_*->fieldname
```

该字符串`->`用于将密钥名称与散列字段名称分开。如上文所述，密钥被替换，并且存储在结果密钥中的散列被访问以检索指定的散列字段。

## 返回值

[数组回复](https://redis.io/topics/protocol#array-reply)：在不传递`store`选项的情况下，该命令返回一个有序元素列表。[整数回复](https://redis.io/topics/protocol#integer-reply)：当`store`指定该选项时，该命令返回目标列表中排序元素的数量。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  