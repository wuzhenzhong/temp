# Linux平台安装MongoDB



## Linux平台安装MongoDB

## 下载

MongoDB提供了linux平台上32位和64位的安装包，你可以在官网下载安装包。

下载地址：[http://www.mongodb.org/downloads](https://www.mongodb.org/downloads)

![img](https://ask.qcloudimg.com/http-save/devdocs/b1gpmwwt4s.png)

## 安装

下载完成后，在你安装的目录下解压zip包。

## 创建数据库目录

MongoDB的数据存储在data目录的db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。

以下实例中我们将data目录创建于根目录下(/)。

注意：/data/db 是 MongoDB 默认的启动的数据库路径(--dbpath)。

```js
mkdir -p /data/db
```

## 命令行中运行 MongoDB 服务

你可以再命令行中执行mongo安装目录中的bin目录执行mongod命令来启动mongdb服务。

> 注意：如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。

```js
$ ./mongod
2015-09-25T16:39:50.549+0800 I JOURNAL  [initandlisten] journal dir=/data/db/journal
2015-09-25T16:39:50.550+0800 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-09-25T16:39:50.869+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 3.16
2015-09-25T16:39:51.206+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 3.52
2015-09-25T16:39:52.775+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 7.7 
```

## MongoDB后台管理 Shell

如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo命令文件。

MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。

当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）：

```js
$ cd /usr/local/mongodb/bin
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
Welcome to the MongoDB shell.
……
```

由于它是一个JavaScript shell，您可以运行一些简单的算术运算:

```js
> 2+2
4
> 3+6
9
```

现在让我们插入一些简单的数据，并对插入的数据进行检索：

```js
> db.W3Cschool.insert({x:10})
WriteResult({ "nInserted" : 1 })
> db.W3Cschool.find()
{ "_id" : ObjectId("5604ff74a274a611b0c990aa"), "x" : 10 }
>
```

第一个命令是将数据 8 插入到w3r集合（表）的 z 字段中。





```js
$ ./mongod --dbpath=/data/db --rest
```







本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01