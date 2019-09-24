# Mysql 事务的隔离级别

阅读 1552

收藏 91

2016-08-17

原文链接：[www.jianshu.com](https://link.juejin.im/?target=http%3A%2F%2Fwww.jianshu.com%2Fp%2F1ffa0d065706)

开发工作中我们会使用到事务，那你们知道事务又分哪几种吗？

MYSQL标准定义了4类隔离级别，用来限定事务内外的哪些改变是可见的，哪些是不可见的。
低的隔离级一般支持更高的并发处理，并拥有更低的系统开销。
隔离级别由低到高：Read Uncommitted < Read Committed < Repeatable Read < Serializable。



> **Read Uncommitted（读取未提交内容）**

在该隔离级别，所有事务都可以看到其他未提交(commit)事务的执行结果。
本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。
读取未提交的数据，也被称之为脏读（Dirty Read）。



```
[窗口A]:

mysql> set GLOBAL tx_isolation='READ-UNCOMMITTED';
Query OK, 0 rows affected (0.00 sec)

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+------------------+
| @@tx_isolation   |
+------------------+
| READ-UNCOMMITTED |
+------------------+
1 row in set (0.00 sec)

mysql> use test;
Database changed
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
+----+------+
2 rows in set (0.00 sec)

[窗口B]:
mysql> select @@tx_isolation;
+------------------+
| @@tx_isolation   |
+------------------+
| READ-UNCOMMITTED |
+------------------+
1 row in set (0.00 sec)

mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into test.user values (3, 'c');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
|  3 | c    |
+----+------+
3 rows in set (0.00 sec)

//目前为止，窗口B并未commit;

[窗口A]:
mysql> select * from user ;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
|  3 | c    |
+----+------+
3 rows in set (0.00 sec)
```

> **Read Committed（读取提交内容）**

这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。
它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。
这种隔离级别 也支持所谓的不可重复读（NonrepeatableRead），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一 select 可能返回不同结果。



```
[窗口A]：

mysql> SET GLOBAL tx_isolation='READ-COMMITTED';
Query OK, 0 rows affected (0.00 sec)

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| READ-COMMITTED |
+----------------+
1 row in set (0.00 sec)

mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
+----+------+
2 rows in set (0.00 sec)

[窗口B]:

mysql> SELECT @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| READ-COMMITTED |
+----------------+
1 row in set (0.00 sec)

mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
+----+------+
2 rows in set (0.00 sec)

mysql> delete from test.user where id=1;
Query OK, 1 row affected (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
+----+------+
1 row in set (0.00 sec)

[窗口A]:

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  1 | a    |
|  2 | b    |
+----+------+
2 rows in set (0.00 sec)

[窗口B]:

mysql> commit;
Query OK, 0 rows affected (0.02 sec)

[窗口A]:

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
+----+------+
1 row in set (0.00 sec)
```

> **Repeatable Read（可重读）**

这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。
不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。
简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。
InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。



```
[窗口A]:

mysql> SET GLOBAL tx_isolation='REPEATABLE-READ';
Query OK, 0 rows affected (0.00 sec)

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+
1 row in set (0.00 sec)

mysql> begin;
Query OK, 0 rows affected (0.00 sec)

[窗口B]:

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+
1 row in set (0.00 sec)

mysql> insert into test.user values (4, 'd');
Query OK, 1 row affected (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
|  4 | d    |
+----+------+
2 rows in set (0.00 sec)

[窗口A]:

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
+----+------+
1 rows in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
|  4 | d    |
+----+------+
2 rows in set (0.00 sec)
```

> **Serializable（序列化执行）**

这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。
简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。 

```
[窗口A]:

mysql> SET GLOBAL tx_isolation='SERIALIZABLE';
Query OK, 0 rows affected (0.00 sec)

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| SERIALIZABLE   |
+----------------+
1 row in set (0.00 sec)

mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
|  4 | d    |
+----+------+
2 rows in set (0.00 sec)

mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into test.user values (5, 'e');
Query OK, 1 row affected (0.00 sec)

[窗口B]:

mysql> quit;
Bye

[root@vagrant-centos65 ~]# mysql -uroot -pxxxx(重新登录)

mysql> SELECT @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| SERIALIZABLE   |
+----------------+
1 row in set (0.00 sec)

mysql> select * from test.user;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

[窗口A]:

mysql> commit;
Query OK, 0 rows affected (0.01 sec)

[窗口B]:

mysql> mysql> select * from test.user;
+----+------+
| id | name |
+----+------+
|  2 | b    |
|  4 | d    |
|  5 | e    |
+----+------+
3 rows in set (0.00 sec)
```

Thanks ~