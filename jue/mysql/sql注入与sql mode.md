# sql注入与sql mode

## **概述**

  sql注入就是利用某些数据库的外接接口将用户数据插入到实际的数据库操作语言当中，从而达到入侵数据库乃至操作系统的目的。在安全领域，`我们永远不要信任用户的输入`，我们必须认定用户输入的数据都是不安全的，我们都需要对用户输入的数据进行过滤处理。`没有（运行时）编译，就没有注入。`所以从根本上防止上述类型攻击的手段，还是避免数据变成代码被执行，时刻分清代码和数据的界限。而具体到SQL注入来说，被执行的恶意代码是通过数据库的SQL解释引擎编译得到的，所以只要避免用户输入的数据被数据库系统编译就可以了。
  与其他数据库不同，MySQL可以运行在不同的SQL Mode(SQL服务器模式)下，并且可以为不同客户端应用不同模式。这样每个应用程序可以根据自己的需求来定制服务器的操作模式。模式定义MySQL应支持哪些SQL语法，以及应执行哪种数据验证检查。这有点类似于apache配置不同级别的错误日志，报告哪些错误，又不报告哪些错误。

## **SQL注入**

### 1.注入实例

```
//php代码
$unsafe_variable = $_POST['user_input'];   
mysql_query("INSERT INTO `table` (`column`) VALUES ('{$unsafe_variable}')");复制代码
```

当post中代码如下时候：

```
value'); DROP TABLE table;--复制代码
```

查询代码变成

```
INSERT INTO `table` (`column`) VALUES('value'); DROP TABLE table;--')复制代码
```

这样会直接删除table表，你的数据被破坏了。

### 2.防止sql注入

**方法一**
prepareStatement+Bind-Variable：SQL语句和查询的参数分别发送给数据库服务器进行解析。
对于php来说有两种实现方式。

```
//使用PDO(PHP data object)
$stmt = $pdo->prepare('SELECT * FROM employees WHERE name = :name');  
$stmt->execute(array('name' => $name));  
foreach ($stmt as $row) {  
    // do something with $row  
}

//使用mysql扩展-mysqli
$stmt = $dbConnection->prepare('SELECT * FROM employees WHERE name = ?');
$stmt->bind_param('s', $name);
$stmt->execute();
$result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    // do something with $row
}复制代码
```

**方式二**
对查询语句进行转义（最常见的方式）：使用应用程序提供的转换函数。
|应用|函数|
|--------|
|MySQL C API|mysql_real_escape_string()|
|MySQL++|escape和quote修饰符|
|PHP|使用mysql_real_escape_string()(适用于PHP4.3.0以前),之后可以使用mysqli或pdo|
|Perl DBI|placeholder或quote()|
|Ruby DBI|placeholder或quote()|

**方式三**
使用自己定义函数进行校验：其本质上还是对输入非法数据进行转义和过滤。
输入验证可以分为：1.整理数据使之有效；2.拒绝已知的非法输入；3.只接收已知的合法输入。

**方式四**
使用存储过程。
存储过程参见：[ (9)mysql中的存储过程和自定义函数](https://link.juejin.im/?target=http%3A%2F%2Fblog.csdn.net%2Fpursuing0my0dream%2Farticle%2Fdetails%2F45047559)

## **sql服务器模式**

### 1.sql模式语法

```
#查看当前sql模式
select @@sql_mode;
#查看当前sql模式
SELECT @@session.sql_mode;
#修改当前sql模式
SET [SESSION][GLOBAL] sql_mode='modes';复制代码
```

- 其中session选项表示只在本次连接生效；而global表示在本次连接不生效，下次连接生效。
- 也可以使用“--sql-mode='modes'”，在MySQL启动时候设置sql_mode。
- 可以在配置文件中设置。
  \###2.sql_mode常用值
  **ONLY_FULL_GROUP_BY：**
  对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么这个SQL是不合法的，因为列不在GROUP BY从句中。

**NO_AUTO_VALUE_ON_ZERO：**
该值影响自增长列的插入。默认设置下，插入0或NULL代表生成下一个自增长值。如果用户 希望插入的值为0，而该列又是自增长的，那么这个选项就有用了。

**STRICT_TRANS_TABLES：**
在该模式下，如果一个值不能插入到一个事务表中，则中断当前的操作，对非事务表不做限制。

**NO_ZERO_IN_DATE：**
在严格模式下，不允许日期和月份为零。

**NO_ZERO_DATE：**
设置该值，mysql数据库不允许插入零日期，插入零日期会抛出错误而不是警告。

**ERROR_FOR_DIVISION_BY_ZERO：**
在INSERT或UPDATE过程中，如果数据被零除，则产生错误而非警告。如 果未给出该模式，那么数据被零除时MySQL返回NULL。

**NO_AUTO_CREATE_USER：**
禁止GRANT创建密码为空的用户。

**NO_ENGINE_SUBSTITUTION：**
如果需要的存储引擎被禁用或未编译，那么抛出错误。不设置此值时，用默认的存储引擎替代，并抛出一个异常。

**PIPES_AS_CONCAT：**
将"||"视为字符串的连接操作符而非或运算符，这和Oracle数据库是一样的，也和字符串的拼接函数Concat相类似。

**ANSI_QUOTES：**
启用ANSI_QUOTES后，不能用双引号来引用字符串，因为它被解释为识别符。

*说明*

> ORACLE的sql_mode设置等同：PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE, NO_KEY_OPTIONS, NO_TABLE_OPTIONS, NO_FIELD_OPTIONS, NO_AUTO_CREATE_USER。

## **参考**

### 1.sql注入

[www.zhihu.com/question/22…](https://link.juejin.im/?target=http%3A%2F%2Fwww.zhihu.com%2Fquestion%2F22953267)
[blog.csdn.net/agoago_2009…](https://link.juejin.im/?target=http%3A%2F%2Fblog.csdn.net%2Fagoago_2009%2Farticle%2Fdetails%2F37884797)

### 2.sql模式

[tech.it168.com/a2012/0822/…](https://link.juejin.im/?target=http%3A%2F%2Ftech.it168.com%2Fa2012%2F0822%2F1388%2F000001388401.shtml)
[c.biancheng.net/cpp/html/14…](https://link.juejin.im/?target=http%3A%2F%2Fc.biancheng.net%2Fcpp%2Fhtml%2F1471.html)