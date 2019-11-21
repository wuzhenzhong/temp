# MongoDB 删除数据库



## MongoDB 删除数据库

### 语法

MongoDB 删除数据库的语法格式如下：

```js
db.dropDatabase()
```

删除当前数据库，默认为 test，你可以使用 db 命令查看当前数据库名。

### 实例

以下实例我们删除了数据库 youj。

首先，查看所有数据库：

```js
> show dbs
local   0.078GB
youj  0.078GB
test    0.078GB
```

接下来我们切换到数据库 youj：

```js
> use youj
switched to db youj
> 
```

执行删除命令：

```js
> db.dropDatabase()
{ "dropped" : "youj", "ok" : 1 }
```

最后，我们再通过 show dbs 命令数据库是否删除成功：

```js
> show dbs
local  0.078GB
test   0.078GB
> 
```

### 删除集合

集合删除语法格式如下：

```js
db.collection.drop()
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01