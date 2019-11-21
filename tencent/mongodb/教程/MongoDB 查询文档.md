# MongoDB 查询文档

## MongoDB 查询文档

### 语法

MongoDB 查询数据的语法格式如下：

```js
>db.COLLECTION_NAME.find()
```

find() 方法以非结构化的方式来显示所有文档。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```js
>db.col.find().pretty()
```

pretty() 方法以格式化的方式来显示所有文档。

### 实例

以下实例我们查询了集合 col 中的数据：

```js
> db.col.find().pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "w3cschool",
        "url" : "http://www.w3cschool.cn",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。

## MongoDB 与 RDBMS Where 语句比较

如果你熟悉常规的 SQL 数据，通过下表可以更好的理解 MongoDB 的条件语句查询：

| 操作       | 格式                   | 范例                                      | RDBMS中的类似语句      |
| :--------- | :--------------------- | :---------------------------------------- | :--------------------- |
| 等于       | {<key>:<value>}        | db.col.find({"by":"w3cschool"}).pretty()  | where by = 'w3cschool' |
| 小于       | {<key>:{$lt:<value>}}  | db.col.find({"likes":{$lt:50}}).pretty()  | where likes < 50       |
| 小于或等于 | {<key>:{$lte:<value>}} | db.col.find({"likes":{$lte:50}}).pretty() | where likes <= 50      |
| 大于       | {<key>:{$gt:<value>}}  | db.col.find({"likes":{$gt:50}}).pretty()  | where likes > 50       |
| 大于或等于 | {<key>:{$gte:<value>}} | db.col.find({"likes":{$gte:50}}).pretty() | where likes >= 50      |
| 不等于     | {<key>:{$ne:<value>}}  | db.col.find({"likes":{$ne:50}}).pretty()  | where likes != 50      |

## MongoDB AND 条件

MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，及常规 SQL 的 AND 条件。

语法格式如下：

```js
>db.col.find({key1:value1, key2:value2}).pretty()
```

### 实例

以下实例通过 **by** 和 **title** 键来查询 **w3cschool** 中 **MongoDB 教程** 的数据

```js
> db.col.find({"by":"w3cschool", "title":"MongoDB 教程"}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "w3cschool",
        "url" : "http://www.w3cschool.cn",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

以上实例中类似于 WHERE 语句：**WHERE by='w3cschool' AND title='MongoDB 教程'**

## MongoDB OR 条件

MongoDB OR 条件语句使用了关键字 **$or**,语法格式如下：

```js
>db.col.find(
   {
      $or: [
	     {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

### 实例

以下实例中，我们演示了查询键 **by** 值为 w3cschool 或键 **title** 值为 **MongoDB 教程** 的文档。

```js
>db.col.find({$or:[{"by":"w3cschool"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "w3cschool",
        "url" : "http://www.w3cschool.cn",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```

## AND 和 OR 联合使用

以下实例演示了 AND 和 OR 联合使用，类似常规 SQL 语句为： **'where likes>50 AND (by = 'w3cschool' OR title = 'MongoDB 教程')'**

```js
>db.col.find({"likes": {$gt:50}, $or: [{"by": "w3cschool"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "w3cschool",
        "url" : "http://www.w3cschool.cn",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com