# 网站建设之SQL 指南

## SQL 指南

------

## SQL - 结构化查询语言 (Structured Query Language)

SQL 是用于访问和处理数据库的标准的计算机语言。

常用的数据库管理系统： MySQL, SQL Server, Access, Oracle, Sybase, 和 DB2

对于那些希望在数据库中存储数据并从中获取数据的人来说，SQL 的知识是价值无法衡量的。

------

## 什么是 SQL?

- SQL 指结构化查询语言 (*S*tructured *Q*uery *L*anguage)
- SQL 使我们有能力访问数据库
- SQL 是一种 ANSI 的标准计算机语言
- SQL 面向数据库执行查询
- SQL 可从数据库取回数据
- SQL 可在数据库中插入新的记录
- SQL 可从数据库删除记录
- SQL 很容易学习

------

## SQL 数据库表

一个数据库通常包含一个或多个表。每个表由一个名字标识（例如"客户"或者"订单"）。表包含带有数据的记录（行）。

下面的例子是一个名为 "Persons" 的表：

| LastName  | FirstName | Address      | City      |
| :-------- | :-------- | :----------- | :-------- |
| Hansen    | Ola       | Timoteivn 10 | Sandnes   |
| Svendson  | Tove      | Borgvn 23    | Sandnes   |
| Pettersen | Kari      | Storgt 20    | Stavanger |

上面的表包含三条记录（每一条对应一个人）和四个列（姓、名、地址和城市）。

------

## SQL 查询程序

通过 SQL，我们可以查询某个数据库，并获得返回的一个结果集。

查询程序类似这样：

 SELECT LastName FROM Persons 

结果集类似这样：

| LastName  |
| :-------- |
| Hansen    |
| Svendson  |
| Pettersen |



------

## 如何学习SQL？

访问我们 [完整的 SQL 教程](https://www.w3cschool.cn/sql)