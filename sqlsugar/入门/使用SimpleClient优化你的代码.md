# 使用SimpleClient优化你的代码



我们之前学会了用 SqlSugarClient对象来操作数据库了，但是对于一些喜欢泛型的人来说，还不够精简，我们就来学习一下SimpleClient



**定义DbContext**

```cs
`public` `class` `DbContext``{``    ``public` `DbContext()``    ``{``        ``Db = ``new` `SqlSugarClient(``new` `ConnectionConfig()``        ``{``            ``ConnectionString = ``"server=.;uid=sa;pwd=@jhl85661501;database=SqlSugar4XTest"``,``            ``DbType = DbType.SqlServer,``            ``InitKeyType = InitKeyType.Attribute,``//从特性读取主键和自增列信息``            ``IsAutoCloseConnection = ``true``,``//开启自动释放模式和EF原理一样我就不多解释了` `        ``});``        ``//调式代码 用来打印SQL ``        ``Db.Aop.OnLogExecuting = (sql, pars) =>``        ``{``            ``Console.WriteLine(sql + ``"\r\n"` `+``                ``Db.Utilities.SerializeObject(pars.ToDictionary(it => it.ParameterName, it => it.Value)));``            ``Console.WriteLine();``        ``};` `    ``}``    ``//注意：不能写成静态的，不能写成静态的``    ``public` `SqlSugarClient Db;``//用来处理事务多表查询和复杂的操作``    ``public` `SimpleClient<Student> StudentDb { ``get` `{ ``return` `new` `SimpleClient<Student>(Db); } }``//用来处理Student表的常用操作``    ``public` `SimpleClient<School> SchoolDb { ``get` `{ ``return` `new` `SimpleClient<School>(Db); } }``//用来处理School表的常用操作``}`
```





**使用DbContext完成增删查改,注意使用 DemoMangar需要new出来使用，保证他的上级或者上上级不能单例或者静态**

```cs
`public` `class` `DemoManager : DbContext``//继承DbContext``{`  `    ``//SimpleClient实现查询例子``    ``public` `void` `SearchDemo()``    ``{` `        ``var` `data1 = StudentDb.GetById(1);``//根据ID查询``        ``var` `data2 = StudentDb.GetList();``//查询所有``        ``var` `data3 = StudentDb.GetList(it => it.Id == 1);  ``//根据条件查询  ``        ``var` `data4 = StudentDb.GetSingle(it => it.Id == 1);``//根据条件查询一条` `        ``var` `p = ``new` `PageModel() { PageIndex = 1, PageSize = 2 };``// 分页查询``        ``var` `data5 = StudentDb.GetPageList(it => it.Name == ``"xx"``, p);``        ``Console.Write(p.PageCount);``//返回总数`  `        ``// 分页查询加排序``        ``var` `data6 = StudentDb.GetPageList(it => it.Name == ``"xx"``, p, it => it.Name, OrderByType.Asc);``        ``Console.Write(p.PageCount);``//返回总数`  `        ``//组装条件查询作为条件实现 分页查询加排序``        ``List<IConditionalModel> conModels = ``new` `List<IConditionalModel>();``        ``conModels.Add(``new` `ConditionalModel() { FieldName = ``"id"``, ConditionalType = ConditionalType.Equal, FieldValue = ``"1"` `});``//id=1``        ``var` `data7 = StudentDb.GetPageList(conModels, p, it => it.Name, OrderByType.Asc);``        ` `        ``//4.9.7.5支持了转换成queryable,我们可以用queryable实现复杂功能``         ``StudentDb.AsQueryable().Where(x => x.Id == 1).ToList();``    ``}`  `    ``//插入例子``    ``public` `void` `InsertDemo()``    ``{` `        ``var` `student = ``new` `Student() { Name = ``"jack"` `};``        ``var` `studentArray = ``new` `Student[] { student };` `        ``StudentDb.Insert(student);``//插入` `        ``StudentDb.InsertRange(studentArray);``//批量插入` `        ``var` `id = StudentDb.InsertReturnIdentity(student);``//插入返回自增列``        ` `        ``//4.9.7.5我们可以转成 Insertable实现复杂插入``        ``StudentDb.AsInsertable(insertObj).ExecuteCommand();``    ``}`  `    ``//更新例子``    ``public` `void` `UpdateDemo()``    ``{``        ``var` `student = ``new` `Student() { Id = 1, Name = ``"jack"` `};``        ``var` `studentArray = ``new` `Student[] { student };` `        ``StudentDb.Update(student);``//根据实体更新` `        ``StudentDb.UpdateRange(studentArray);``//批量更新` `        ``StudentDb.Update(it => ``new` `Student() { Name = ``"a"``, CreateTime = DateTime.Now }, it => it.Id == 1);``// 只更新Name列和CreateTime列，其它列不更新，条件id=1``    ` `        ``//支持StudentDb.AsUpdateable(student)``    ``}` `    ``//删除例子``    ``public` `void` `DeleteDemo()``    ``{``        ``var` `student = ``new` `Student() { Id = 1, Name = ``"jack"` `};` `        ``StudentDb.Delete(student);``//根据实体删除``        ``StudentDb.DeleteById(1);``//根据主键删除``        ``StudentDb.DeleteById(``new` `int``[] { 1,2});``//根据主键数组删除``        ``StudentDb.Delete(it=>it.Id==1);``//根据条件删除``        ` `        ``//支持StudentDb.AsDeleteable()``    ``}` `    ``//使用事务的例子``    ``public` `void` `TranDemo()``    ``{` `        ``var` `result = Db.Ado.UseTran(() =>``        ``{``            ``//这里写你的逻辑``        ``});``        ``if` `(result.IsSuccess)``        ``{``            ``//成功``        ``}``        ``else``        ``{``            ``Console.WriteLine(result.ErrorMessage);``        ``}``    ``}` `}`
```



对于多表查询我们这儿不细讲用法如下

```cs
`//多表查询``public` `void` `JoinDemo() {` `        ``var` `list= Db.Queryable<Student, School>((st, sc) => ``new` `object``[] {``            ``JoinType.Left,``            ``st.SchoolId==sc.Id``        ``}).Select<ViewModelStudent>().ToList();``}`
```

