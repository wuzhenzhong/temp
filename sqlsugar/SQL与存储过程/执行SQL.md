# 执行SQL

**Sqlqueryable**

sqlueryable只支持查询操作，并且支持拉姆达分页

```cs
var t12 = db.SqlQueryable<Student>("select * from student").Where(it=>it.id>0).ToPageList(1, 2);
var t12 = db.SqlQueryable<dynamic>("select * from student").ToPageList(1, 2);//返回动态类型
```



**Ado方法**

**1.重载：object parameters**

```cs
var dt=db.Ado.SqlQuery<Student>("select * from table where id=@id and name=@name",new {id=1,name="a"});
var dt=db.Ado.SqlQuery<Student>("select * from table where id=@id and name=@name",字典);
```

**2.重载： List<SugarParameter> parameters**

```cs
var dt=db.Ado.GetDataTable("select * from table where id=@id and name=@name",new List<SugarParameter>(){
  new SugarParameter("@id",1),
  new SugarParameter("@name",2)
});
```

**3.重载：params SugarParameter[] parameters**

```cs
var dt=db.Ado.GetDataTable("select * from table");
 
var dt=db.Ado.GetDataTable("select * from table where id=@id",new SugarParameter("@id",1));
 
var dt=db.Ado.GetDataTable("select * from table where id=@id and name=@name",new SugarParameter []{
  new SugarParameter("@id",1),
  new SugarParameter("@name",2)
});
```

# 全部函数



1.获取DataTable (如是.Net Core版本, DataTable是Sqlsugar自定义的DataTable, 因为以前的Core 1.x不支持DataTable, 后遗症, 效率不用担心)

```cs
db.Ado.GetDataTable(string sql, object parameters);
db.Ado.GetDataTable(string sql, params SugarParameter[] parameters);
db.Ado.GetDataTable(string sql, List<SugarParameter> parameters);
```



2.获取DataSet

```cs
db.Ado.GetDataSetAll(string sql, object parameters);
db.Ado.GetDataSetAll(string sql, params SugarParameter[] parameters);
db.Ado.GetDataSetAll(string sql, List<SugarParameter> parameters);
```



3.获取DataReader

```cs
db.Ado.GetDataReader(string sql, object parameters);
db.Ado.GetDataReader(string sql, params SugarParameter[] parameters);
db.Ado.GetDataReader(string sql, List<SugarParameter> parameters);
```



4.获取首行首列返回object类型

```cs
db.Ado.GetScalar(string sql, object parameters);
db.Ado.GetScalar(string sql, params SugarParameter[] parameters);
db.Ado.GetScalar(string sql, List<SugarParameter> parameters);
```



5.执行数据库返回受影响行数

```cs
int ExecuteCommand(string sql, object parameters);
int ExecuteCommand(string sql, params SugarParameter[] parameters);
int ExecuteCommand(string sql, List<SugarParameter> parameters);
```



6.获取首行首列更多重载

```cs
//以下为返回string
string GetString(string sql, object parameters);
string GetString(string sql, params SugarParameter[] parameters);
string GetString(string sql, List<SugarParameter> parameters);
 
//返回int
int GetInt(string sql, object pars);
int GetInt(string sql, params SugarParameter[] parameters);
int GetInt(string sql, List<SugarParameter> parameters);
 
//返回double
db.Ado.GetDouble(string sql, object parameters);
db.Ado.GetDouble(string sql, params SugarParameter[] parameters);
db.Ado.GetDouble(string sql, List<SugarParameter> parameters);
 
//返回decimal
db.Ado.GetDecimal(string sql, object parameters);
db.Ado.GetDecimal(string sql, params SugarParameter[] parameters);
db.Ado.GetDecimal(string sql, List<SugarParameter> parameters);
 
//返回DateTime
db.Ado.GetDateTime(string sql, object parameters);
db.Ado.GetDateTime(string sql, params SugarParameter[] parameters);
db.Ado.GetDateTime(string sql, List<SugarParameter> parameters);
```



7.查询并返回List<T>

```cs
db.Ado.SqlQuery<T>(string sql, object whereObj = null);
db.Ado.SqlQuery<T>(string sql, params SugarParameter[] parameters);
db.Ado.SqlQuery<T>(string sql, List<SugarParameter> parameters);
```



8.查询返回单条记录

```cs
db.Ado.SqlQuerySingle<T>(string sql, object whereObj = null);
db.Ado.SqlQuerySingle<T>(string sql, params SugarParameter[] parameters);
db.Ado.SqlQuerySingle<T>(string sql, List<SugarParameter> parameters);
```



9.查询返回动态类型(该类型为Newtonsoft.Json里面的JObject类型, 使用方法自行百度)

```cs
db.Ado.SqlQueryDynamic(string sql, object whereObj = null);
db.Ado.SqlQueryDynamic(string sql, params SugarParameter[] parameters);
db.Ado.SqlQueryDynamic(string sql, List<SugarParameter> parameters);
```