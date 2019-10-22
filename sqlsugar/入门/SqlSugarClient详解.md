# SqlSugarClient详解

当你对SqlSugar有一定了解后可以在看下面的内容



**创建数据库连接对象**

SqlSugarClient是通过参数ConnectionConfig进行创建的，ConnectionConfig有6个属性分别是：

```cs
`SqlSugarClient db = ``new` `SqlSugarClient(``new` `ConnectionConfig()``{ ``  ``ConnectionString = Config.ConnectionString,``//必填, 数据库连接字符串``  ``DbType = DbType.SqlServer,        　``//必填, 数据库类型``  ``IsAutoCloseConnection = ``true``，       ``//默认false, 时候知道关闭数据库连接, 设置为true无需使用using或者Close操作``  ``InitKeyType = InitKeyType.SystemTable    ``//默认SystemTable, 字段信息读取, 如：该属性是不是主键，是不是标识列等等信息``});` `List<Student> list=db.Queryable<Student>().ToList();``//查询所有（使用SqlSugarClient查询所有到LIST）`
```



**1.ConnectionString**：连接字符串



**2.DataType**: 数据库类型



**3.IsAutoCloseConnection**：(默认false)是否自动释放数据库，设为true我们不需要close或者Using的操作，比较推荐



**4.InitKeyType**：会影响你定义的实体具体查看 http://www.codeisbug.com/Doc/8/1141



**5.MoreSettings** (4.6.2)

用于一些全局设置

MoreSettings .IsAutoRemoveDataCache 为true表示可以自动删除二级缓存

MoreSettings .IsWithNoLockQuery 为true表式查询的时候默认会加上.With(SqlWith.NoLock)，可以用With(SqlWith.Null)让全局的失效



**6.ConfigureExternalServices** 

1 SerializeService 扩展你想要的序列化方式 

2 ReflectionInoCacheService缓存方式 

3 AppendDataReaderTypeMappings 自定义数据类型

4 SqlFuncServices自定义拉姆达 

5 EntityService 如果不想用SqlSugar里面的 实体特性可以用这个自定义实现



**7.SlaveConnectionConfigs**

主从模式配置



注意：SqlSugarClient 不能跨线程使用，保证一个线程new 一个SqlSugarClient  ，最典型的例子有人直接把他设成了静态变量，这样做是错误的。



如果要使用静态属性, 这种写法才是正确的:

```cs
`public` `static` `SqlSugarClient DB``{``    ``get` `=> ``new` `SqlSugarClient(``new` `ConnectionConfig()``    ``{ ``      ``ConnectionString = Config.ConnectionString,``//必填, 数据库连接字符串``      ``DbType = DbType.SqlServer,        　``//必填, 数据库类型``      ``IsAutoCloseConnection = ``true``，       ``//默认false, 时候知道关闭数据库连接, 设置为true无需使用using或者Close操作``      ``InitKeyType = InitKeyType.SystemTable    ``//默认SystemTable, 字段信息读取, 如：该属性是不是主键，是不是标识列等等信息``    ``});``}`
```







Mysql 注意：

如果发现数据量大分页慢请升级你的数据库，高版本会得到解决，具体哪个版本不记得了不要太低就好



Oracle注意：

完美支持序列 在实体特性可以设置序列，设置完后可以当自增列返回



SqlServer注意：

sql08以上的批量操作性能达到了Sqlbluecopy水平，如果性能要求高建议升级

08及以下只比循环快些不及Sqlbluecopy



Sqlite注意

可以满足正常的Sqlite数据类型，如果是非正常的数据类型ORM不支持可以自已添加扩展

```cs
`SqlSugarClient db = ``new` `SqlSugarClient(``new` `ConnectionConfig() {``                 ``ConfigureExternalServices=``new` `ConfigureExternalServices()``                 ``{``                      ``AppendDataReaderTypeMappings=``new` `List<KeyValuePair<``string``, CSharpDataType>>() {``                           ``new` `KeyValuePair<``string``, CSharpDataType>(``"int32"``,CSharpDataType.@``int``),``                      ``}` `                 ``} ,`` ``ConnectionString = Config.ConnectionString, DbType = DbType.Sqlite, IsAutoCloseConnection = ``true` `});`
```





# 数据库连接池设置

我们可以在连接字符串中设置连接池，默认值为100 



pooling=true; --表示开启连接池（默认为开启)

min pool size = 2 --最小连接池大小：即什么也没执行初次连接的时候先和数据库服务建立n个连接

max pool size=4 --最大连接池大小：允许建立的最大连接数，是在需要的时候建立。

举例说明：min pool size = 2;max pool size=4 ; 