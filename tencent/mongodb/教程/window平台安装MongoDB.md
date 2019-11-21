# window平台安装MongoDB

## window平台安装 MongoDB

## MongoDB 下载

MongoDB提供了可用于32位和64位系统的预编译二进制包，你可以从MongoDB官网下载安装，MongoDB预编译二进制包下载地址：

[http://www.mongodb.org/downloads](https://www.mongodb.org/downloads)

![img](https://ask.qcloudimg.com/http-save/devdocs/o18i35pdrr.png)

## 解压

下载zip包后，解压安装包，并安装它。

**创建数据目录**

MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。请注意，数据目录应该创建在根目录下（(如： C:\ 或者 D:\ 等 )。

在本教程中，我们已经在D：盘中解压了mongodb文件，现在让我们创建一个data的目录然后在data目录里创建db目录。

![img](https://ask.qcloudimg.com/http-save/devdocs/6h1ny6cudd.png)

你也可以通过window的资源管理器中创建这些目录，而不一定通过命令行。

## 命令行下运行 MongoDB 服务器

为了从命令提示符下运行MongoDB服务器，你必须从MongoDB目录的bin目录中执行mongod.exe文件。

![img](https://ask.qcloudimg.com/http-save/devdocs/k59f76qx8j.png)

## 将MongoDB服务器作为Windows服务运行

请注意，你必须有管理权限才能运行下面的命令。执行以下命令将MongoDB服务器作为Windows服务运行：

```js
mongod --bind_ip yourIPadress --logpath "C:\data\dbConf\mongodb.log" --logappend --dbpath "C:\data\db" --port yourPortNumber --serviceName "YourServiceName" --serviceDisplayName "YourServiceName" --install
```

**下表为mongodb启动的参数说明：**

| 参数                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| --bind_ip           | 绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP |
| --logpath           | 定MongoDB日志文件，注意是指定文件不是目录                    |
| --logappend         | 使用追加的方式写日志                                         |
| --dbpath            | 指定数据库路径                                               |
| --port              | 指定服务端口号，默认端口27017                                |
| --serviceName       | 指定服务名称                                                 |
| --serviceDisplayNam | 指定服务名称，有多个mongodb服务时执行。                      |
| --install           | 指定作为一个Windows服务安装。                                |

## MongoDB后台管理 Shell

如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo.exe文件，MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。

当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）：

![img](https://ask.qcloudimg.com/http-save/devdocs/7qux8ivfa3.png)

由于它是一个JavaScript shell，您可以运行一些简单的算术运算:

![img](https://ask.qcloudimg.com/http-save/devdocs/tk914ney7w.png)

db 命令先了当前操作的文档（数据库）：

![img](https://ask.qcloudimg.com/http-save/devdocs/6k90h91ixd.png)

插入一些简单的记录并查找它：

![img](https://ask.qcloudimg.com/http-save/devdocs/05rl4qqf4r.png)

第一个命令将10插入到w3r集合的x字段中

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01