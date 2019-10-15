# 实用 SQL 语句收藏这篇就够了

## 前言

文章沿着设计一个假想的应用 `awesome_app` 为主线，从零创建修改数据库，表格，字段属性，索引，字符集，默认值，自增，增删改查，多表查询，内置函数等实用 SQL 语句。收藏此文，告别零散又低效地搜索经常使用的 SQL 语句。所有 SQL 都在 MySQL 下通过验证，可留着日后回顾参考，也可跟着动手一起做，如果未安装 MySQL 可参考 [《macOS 安装 mysql》](https://juejin.im/post/5da13c38e51d4577ec4eb983) (windows 安装大同小异)。

## 1. 创建

### 1.1 创建数据库

语法：**create database** *db_name*

示例：创建应用数据库 `awesome_app`

```
create database `awesome_app`
复制代码
```

### 1.2 创建表格

语法：**create table** *table_name ( ... columns )*

示例：创建用户表 `users`

```
create table `users`
(
    `id` int,
    `name` char(10),
    `avatar` varchar(300),
    `regtime` date
)
复制代码
```

### 1.3 创建索引

语法：**create index** *index_name* **on** *table_name* (column_name)

示例：为用户 `id` 创建索引 `idx_id`

```
create index `idx_id` on `users` (`id`)
/* 创建唯一索引 */
create unique index `idx_id` on `users` (`id`)
复制代码
```

### 1.4 为已存在的列创建主键

> 更常用的方式是在创建表语句所有列定义的后面添加一行 `primary key (column_name)`。

语法：**alter table** *table_name* **add primary key** (*column_name*)

示例：将用户 `id` 设为主键

```
alter table users add primary key (`id`)
复制代码
```

### 1.5 为已存在的列创建自增约束

> 更常用的方式是在创建表语句中添加自增列 `id int not null auto_increment`。

```
alter table `users` modify `id` int not null auto_increment
复制代码
```

## 2. 插入

语法：

- **insert into** *table_name* **values** (*value1, value2, ...*)
- **insert into** *table_name* (*column1, column2, ...*) **values** (*value1, value2, ...*)

示例：新增注册用户

```
insert into `users` values (1, 'ken', 'http://cdn.awesome_app.com/path/to/xxx/avatar1.jpg', curdate())
/* 指定列插入 */
insert into `users` (`name`, `avatar`) values ('bill', 'http://cdn.awesome_app.com/path/to/xxx/avatar2.jpg')
复制代码
```

## 3. 修改

### 3.1 修改数据记录

语法：

- **update** *table_name* **set** *column=new_value* **where** *condition*
- **update** *table_name* **set** *column1=new_value1,column2=new_value2,...* **where** *condition*

示例：

```
update `users` set `regtime`=curdate() where `regtime` is null
/* 一次修改多列 */
update `users` set `name`='steven',`avatar`='http://cdn.awesome_app.com/path/to/xxx/steven.jpg' where `id`=1
复制代码
```

### 3.2 修改数据库字符集为 `utf8`

```
alter database `awesome_app` default character set utf8
复制代码
```

### 3.3 修改表字符集为 `utf8`

```
alter table `users` convert to character set utf8
复制代码
```

### 3.4 修改表字段字符集为 `utf8`

```
alter table `users` modify `name` char(10) character set utf8
复制代码
```

### 3.5 修改字段类型

```
alter table `users` modify `regtime` datetime not null
复制代码
```

### 3.5 修改字段默认值

```
alter table `users` alter `regtime` set default '2019-10-12 00:00:00'
/* 设置默认为当前时间 current_timestamp，需要重新定义整个列 */
alter table `users` modify `regtime` datetime not null default current_timestamp
复制代码
```

### 3.6 修改字段注释

```
alter table `users` modify `id` int not null auto_increment comment '用户ID';
alter table `users` modify `name` char(10) comment '用户名';
alter table `users` modify `avatar` varchar(300) comment '用户头像';
alter table `users` modify `regtime` datetime not null default current_timestamp comment '注册时间';
复制代码
```

修改后，查看改动后的列：

```
mysql> show full columns from users;
+---------+--------------+-----------------+------+-----+-------------------+----------------+---------------------------------+--------------+
| Field   | Type         | Collation       | Null | Key | Default           | Extra          | Privileges                      | Comment      |
+---------+--------------+-----------------+------+-----+-------------------+----------------+---------------------------------+--------------+
| id      | int(11)      | NULL            | NO   | PRI | NULL              | auto_increment | select,insert,update,references | 用户ID       |
| name    | char(10)     | utf8_general_ci | YES  |     | NULL              |                | select,insert,update,references | 用户名       |
| avatar  | varchar(300) | utf8_general_ci | YES  |     | NULL              |                | select,insert,update,references | 用户头像     |
| regtime | datetime     | NULL            | NO   |     | CURRENT_TIMESTAMP |                | select,insert,update,references | 注册时间     |
+---------+--------------+-----------------+------+-----+-------------------+----------------+---------------------------------+--------------+
复制代码
```

## 4. 删除

### 4.1 删除数据记录

语法：**delete from** *table_name* **where** *condition*

示例：删除用户名未填写的用户

```
# 先增加一条用户名为空的用户
mysql> insert into `users` (`regtime`) values (curdate());
mysql> select * from users;
+----+--------+----------------------------------------------------+------------+
| id | name   | avatar                                             | regtime    |
+----+--------+----------------------------------------------------+------------+
|  1 | steven | http://cdn.awesome_app.com/path/to/xxx/steven.jpg  | 2019-10-12 |
|  2 | bill   | http://cdn.awesome_app.com/path/to/xxx/avatar2.jpg | 2019-10-12 |
|  3 | NULL   | NULL                                               | 2019-10-12 |
+----+--------+----------------------------------------------------+------------+
# 删除用户名为空的行
mysql> delete from `users` where `name` is null;
mysql> select * from users;
+----+--------+----------------------------------------------------+------------+
| id | name   | avatar                                             | regtime    |
+----+--------+----------------------------------------------------+------------+
|  1 | steven | http://cdn.awesome_app.com/path/to/xxx/steven.jpg  | 2019-10-12 |
|  2 | bill   | http://cdn.awesome_app.com/path/to/xxx/avatar2.jpg | 2019-10-12 |
+----+--------+----------------------------------------------------+------------+
复制代码
```

### 4.2 删除数据库

```
drop database if exists `awesome_app`
复制代码
```

### 4.3 删除表

```
drop table if exists `users`
复制代码
```

### 4.4 清空表中所有数据

这个操作相当于先 `drop table` 再 `create table` ，因此需要有 `drop` 权限。

```
truncate table `users`
复制代码
```

### 4.5 删除索引

```
drop index `idx_id` on `users`
复制代码
```

## 5. 查询

### 5.1 语法

```
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
      [HIGH_PRIORITY]
      [STRAIGHT_JOIN]
      [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
      [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr ...]
    [FROM table_references
      [PARTITION partition_list]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [PROCEDURE procedure_name(argument_list)]
    [INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
      | INTO DUMPFILE 'file_name'
      | INTO var_name [, var_name]]
    [FOR UPDATE | LOCK IN SHARE MODE]]
复制代码
```

### 5.2 单表查询

### 5.2.1 准备数据：

```
insert into users (`name`, `avatar`) values
('张三', 'http://cdn.awesome_app.com/path/to/xxx/3.jpg'),
('李四', 'http://cdn.awesome_app.com/path/to/xxx/4.jpg'),
('王五', 'http://cdn.awesome_app.com/path/to/xxx/5.jpg'),
('马六', 'http://cdn.awesome_app.com/path/to/xxx/6.jpg'),
('肖七', 'http://cdn.awesome_app.com/path/to/xxx/7.jpg'),
('刘八', 'http://cdn.awesome_app.com/path/to/xxx/8.jpg'),
('杨九', 'http://cdn.awesome_app.com/path/to/xxx/9.jpg'),
('郑十', 'http://cdn.awesome_app.com/path/to/xxx/10.jpg');

/* 增加重复行 */
insert into users (`name`, `avatar`) values
('张三', 'http://cdn.awesome_app.com/path/to/xxx/3.jpg'),
('李四', 'http://cdn.awesome_app.com/path/to/xxx/4.jpg'),
('王五', 'http://cdn.awesome_app.com/path/to/xxx/5.jpg');
复制代码
```

### 5.2.2 查询所有列

```
mysql> select * from users;
+----+--------+----------------------------------------------------+---------------------+
| id | name   | avatar                                             | regtime             |
+----+--------+----------------------------------------------------+---------------------+
|  1 | steven | http://cdn.awesome_app.com/path/to/xxx/steven.jpg  | 2019-10-12 00:00:00 |
|  2 | bill   | http://cdn.awesome_app.com/path/to/xxx/avatar2.jpg | 2019-10-12 00:00:00 |
|  3 | 张三   | http://cdn.awesome_app.com/path/to/xxx/3.jpg       | 2019-10-13 10:58:37 |
|  4 | 李四   | http://cdn.awesome_app.com/path/to/xxx/4.jpg       | 2019-10-13 10:58:37 |
|  5 | 王五   | http://cdn.awesome_app.com/path/to/xxx/5.jpg       | 2019-10-13 10:58:37 |
|  6 | 马六   | http://cdn.awesome_app.com/path/to/xxx/6.jpg       | 2019-10-13 10:58:37 |
|  7 | 肖七   | http://cdn.awesome_app.com/path/to/xxx/7.jpg       | 2019-10-13 10:58:37 |
|  8 | 刘八   | http://cdn.awesome_app.com/path/to/xxx/8.jpg       | 2019-10-13 10:58:37 |
|  9 | 杨九   | http://cdn.awesome_app.com/path/to/xxx/9.jpg       | 2019-10-13 10:58:37 |
| 10 | 郑十   | http://cdn.awesome_app.com/path/to/xxx/10.jpg      | 2019-10-13 10:58:37 |
| 11 | 张三   | http://cdn.awesome_app.com/path/to/xxx/3.jpg       | 2019-10-13 11:20:17 |
| 12 | 李四   | http://cdn.awesome_app.com/path/to/xxx/4.jpg       | 2019-10-13 11:20:17 |
| 13 | 王五   | http://cdn.awesome_app.com/path/to/xxx/5.jpg       | 2019-10-13 11:20:17 |
+----+--------+----------------------------------------------------+---------------------+
复制代码
```

### 5.2.3 查询指定列

```
mysql> select id,name from users;
+----+--------+
| id | name   |
+----+--------+
|  1 | steven |
|  2 | bill   |
|  3 | 张三   |
|  4 | 李四   |
|  5 | 王五   |
|  6 | 马六   |
|  7 | 肖七   |
|  8 | 刘八   |
|  9 | 杨九   |
| 10 | 郑十   |
| 11 | 张三   |
| 12 | 李四   |
| 13 | 王五   |
+----+--------+
复制代码
```

### 5.2.4 查询不重复记录

```
mysql> select distinct name,avatar  from users;
+--------+----------------------------------------------------+
| name   | avatar                                             |
+--------+----------------------------------------------------+
| steven | http://cdn.awesome_app.com/path/to/xxx/steven.jpg  |
| bill   | http://cdn.awesome_app.com/path/to/xxx/avatar2.jpg |
| 张三   | http://cdn.awesome_app.com/path/to/xxx/3.jpg       |
| 李四   | http://cdn.awesome_app.com/path/to/xxx/4.jpg       |
| 王五   | http://cdn.awesome_app.com/path/to/xxx/5.jpg       |
| 马六   | http://cdn.awesome_app.com/path/to/xxx/6.jpg       |
| 肖七   | http://cdn.awesome_app.com/path/to/xxx/7.jpg       |
| 刘八   | http://cdn.awesome_app.com/path/to/xxx/8.jpg       |
| 杨九   | http://cdn.awesome_app.com/path/to/xxx/9.jpg       |
| 郑十   | http://cdn.awesome_app.com/path/to/xxx/10.jpg      |
+--------+----------------------------------------------------+
复制代码
```

### 5.2.5 限制查询行数

**查询前几行**

```
mysql> select id,name from users limit 2;
+----+--------+
| id | name   |
+----+--------+
|  1 | steven |
|  2 | bill   |
+----+--------+
复制代码
```

**查询从指定偏移(第一行为偏移为0)开始的几行**

```
mysql> select id,name from users limit 2,3;
+----+--------+
| id | name   |
+----+--------+
|  3 | 张三   |
|  4 | 李四   |
|  5 | 王五   |
+----+--------+
复制代码
```

### 5.2.6 排序

```
# 正序
mysql> select distinct name from users order by name asc limit 3;
+--------+
| name   |
+--------+
| bill   |
| steven |
| 刘八   |
+--------+
# 倒序
mysql> select id,name from users order by id desc limit 3;
+----+--------+
| id | name   |
+----+--------+
| 13 | 王五   |
| 12 | 李四   |
| 11 | 张三   |
+----+--------+
复制代码
```

### 5.2.7 分组

**增加城市字段**

```
alter table `users` add `city` varchar(10) comment '用户所在城市' after `name`;
update `users` set `city`='旧金山' where `id`=1;
update `users` set `city`='西雅图' where `id`=2;
update `users` set `city`='北京' where `id` in (3,5,7);
update `users` set `city`='上海' where `id` in (4,6,8);
update `users` set `city`='广州' where `id` between 9 and 10;
update `users` set `city`='深圳' where `id` between 11 and 13;
复制代码
```

**按城市分组统计用户数**

```
mysql> select city, count(name) as num_of_user from users group by city;
+-----------+-------------+
| city      | num_of_user |
+-----------+-------------+
| 上海      |           3 |
| 北京      |           3 |
| 广州      |           2 |
| 旧金山    |           1 |
| 深圳      |           3 |
| 西雅图    |           1 |
+-----------+-------------+
mysql> select city, count(name) as num_of_user from users group by city having num_of_user=1;
+-----------+-------------+
| city      | num_of_user |
+-----------+-------------+
| 旧金山    |           1 |
| 西雅图    |           1 |
+-----------+-------------+
mysql> select city, count(name) as num_of_user from users group by city having num_of_user>2;
+--------+-------------+
| city   | num_of_user |
+--------+-------------+
| 上海   |           3 |
| 北京   |           3 |
| 深圳   |           3 |
+--------+-------------+
复制代码
```

### 5.3 多表关联查询

#### 5.3.1 准备数据

```
create table if not exists `orders`
(
    `id` int not null primary key auto_increment comment '订单ID',
    `title` varchar(50) not null comment '订单标题',
    `user_id` int not null comment '用户ID',
    `cretime` timestamp not null default current_timestamp comment '创建时间'
);
create table if not exists `groups`
(
    `id` int not null primary key auto_increment comment '用户组ID',
    `title` varchar(50) not null comment '用户组标题',
    `cretime` timestamp not null default current_timestamp comment '创建时间'
);
alter table `users` add `group_id` int comment '用户分组' after `city`;

insert into `groups` (`title`) values ('大佬'), ('萌新'), ('菜鸡');
insert into `orders` (`title`, `user_id`) values ('《大佬是怎样炼成的？》', 3), ('《MySQL 从萌新到删库跑路》', 6), ('《菜鸡踩坑记》', 9);
update `users` set `group_id`=1 where `id` between 1 and 2;
update `users` set `group_id`=2 where `id` in (4, 6, 8, 10, 12);
update `users` set `group_id`=3 where `id` in (3, 5, 13);
复制代码
```

#### 5.3.2 join

**join**

用于在多个表中查询相互匹配的数据。

```
mysql> select `users`.`name` as `user_name`, `orders`.`title` as `order_title` from `users`, `orders` where `orders`.`user_id`=`users`.`id`;
+-----------+--------------------------------------+
| user_name | order_title                          |
+-----------+--------------------------------------+
| 张三      | 《大佬是怎样炼成的？》               |
| 马六      | 《MySQL 从萌新到删库跑路》           |
| 杨九      | 《菜鸡踩坑记》                       |
+-----------+--------------------------------------+
复制代码
```

**inner join**

内部连接。效果与 `join` 一样 , 但用法不同，`join` 使用 `where` ，`inner join` 使用 `on` 。

```
mysql> select `users`.`name` as `user_name`, `orders`.`title` as `order_title` from `users` inner join `orders` on `orders`.`user_id`=`users`.`id`;
+-----------+--------------------------------------+
| user_name | order_title                          |
+-----------+--------------------------------------+
| 张三      | 《大佬是怎样炼成的？》               |
| 马六      | 《MySQL 从萌新到删库跑路》           |
| 杨九      | 《菜鸡踩坑记》                       |
+-----------+--------------------------------------+
复制代码
```

**left join**

左连接。返回**左表**所有行，即使**右表**中没有匹配的行，不匹配的用 `NULL` 填充。

```
mysql> select `users`.`name` as `user_name`, `orders`.`title` as `order_title` from `users` left join `orders` on `orders`.`user_id`=`users`.`id`;
+-----------+--------------------------------------+
| user_name | order_title                          |
+-----------+--------------------------------------+
| 张三      | 《大佬是怎样炼成的？》               |
| 马六      | 《MySQL 从萌新到删库跑路》           |
| 杨九      | 《菜鸡踩坑记》                       |
| steven    | NULL                                 |
| bill      | NULL                                 |
| 李四      | NULL                                 |
| 王五      | NULL                                 |
| 肖七      | NULL                                 |
| 刘八      | NULL                                 |
| 郑十      | NULL                                 |
| 张三      | NULL                                 |
| 李四      | NULL                                 |
| 王五      | NULL                                 |
+-----------+--------------------------------------+
复制代码
```

**right join**

右连接。和 `left join` 正好相反，会返回**右表**所有行，即使**左表**中没有匹配的行，不匹配的用 `NULL` 填充。

```
mysql> select `groups`.`title` as `group_title`, `users`.`name` as `user_name` from `groups` right join `users` on `users`.`group_id`=`groups`.`id`;
+-------------+-----------+
| group_title | user_name |
+-------------+-----------+
| 大佬        | steven    |
| 大佬        | bill      |
| 萌新        | 李四      |
| 萌新        | 马六      |
| 萌新        | 刘八      |
| 萌新        | 郑十      |
| 萌新        | 李四      |
| 菜鸡        | 张三      |
| 菜鸡        | 王五      |
| 菜鸡        | 王五      |
| NULL        | 肖七      |
| NULL        | 杨九      |
| NULL        | 张三      |
+-------------+-----------+
复制代码
```

#### 5.3.3 union

`union` 用于合并两个或多个查询结果，合并的查询结果必须具有相同数量的列，并且列拥有形似的数据类型，同时列的顺序相同。

```
mysql> (select `id`, `title` from `groups`) union (select `id`, `title` from `orders`);
+----+--------------------------------------+
| id | title                                |
+----+--------------------------------------+
|  1 | 大佬                                 |
|  2 | 萌新                                 |
|  3 | 菜鸡                                 |
|  1 | 《大佬是怎样炼成的？》               |
|  2 | 《MySQL 从萌新到删库跑路》           |
|  3 | 《菜鸡踩坑记》                       |
+----+--------------------------------------+
复制代码
```

## 6. 函数

### 6.1 语法

**select function**(*column*) **from** *table_name*

### 6.2 合计函数（Aggregate functions）

合计函数的操作面向一系列的值，并返回一个单一的值。通常与 `group by` 语句一起用。

| 函数          | 描述                             |
| :------------ | :------------------------------- |
| avg(column)   | 返回某列的平均值                 |
| count(column) | 返回某列的行数（不包括 NULL 值） |
| count(*)      | 返回被选行数                     |
| first(column) | 返回在指定的域中第一个记录的值   |
| last(column)  | 返回在指定的域中最后一个记录的值 |
| max(column)   | 返回某列的最高值                 |
| min(column)   | 返回某列的最低值                 |
| sum(column)   | 返回某列的总和                   |

### 6.3 标量函数（Scalar functions）

| 函数                      | 描述                           |
| :------------------------ | :----------------------------- |
| ucase(c)                  | 转换为大写                     |
| lcase(c)                  | 转换为小写                     |
| mid(c, start[, end])      | 从文本提取字符                 |
| len(c)                    | 返回文本长度                   |
| instr(c, char)            | 返回在文本中指定字符的数值位置 |
| left(c, number_of_char)   | 返回文本的左侧部分             |
| right(c, number_of_char)  | 返回文本的右侧部分             |
| round(c, decimals)        | 对数值指定小数位数四舍五入     |
| mod(x, y)                 | 取余（求模）                   |
| now()                     | 返回当前的系统日期             |
| format(c, format)         | 格式化显示                     |
| datediff(d, date1, date2) | 日期计算                       |

------

如果未安装 MySQL 可参考 [《macOS 安装 mysql》](https://juejin.im/post/5da13c38e51d4577ec4eb983) (windows 安装大同小异)。