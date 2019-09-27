# .NET Core实战项目之CMS 第五章 入门篇-Dapper的快速入门看这篇就够了

2018.12.05 22:17:00字数 2630阅读 223

## 写在前面

上篇文章我们讲了如在在实际项目开发中使用Git来进行代码的版本控制，当然介绍的都是比较常用的功能。今天我再带着大家一起熟悉下一个ORM框架Dapper，实例代码的演示编写完成后我会通过Git命令上传到GitHub上，正好大家可以再次熟悉下Git命令的使用，来巩固上篇文章的知识。本篇文章已经收入[.NET Core实战项目之CMS 第一章 入门篇-开篇及总体规划](https://www.cnblogs.com/yilezhu/p/9977862.html) 有兴趣的朋友可以加入.NET Core项目实战交流群637326624 进行交流。

> 作者：依乐祝
>
> 原文地址：https://www.cnblogs.com/yilezhu/p/10024091.html

## Dapper是什么

Dapper是.NET下一个轻量级的ORM框架，它和Entity Framework或Nhibnate不同，属于轻量级的，并且是半自动的。也就是说实体类都要自己写。它没有复杂的配置文件，一个单文件就可以了。Dapper通过扩展你的IDbConnection来进行工作的。如果你想了解更多内容的话请点击[这里](https://github.com/StackExchange/Dapper)。

## Dapper快速入门

前面几篇文章我们进行介绍的时候都是手动在代码里面创建的模拟数据，这篇文章我们就结合Dapper来从数据库进行相关的操作。为了演示的方便，这里的实例代码我们就使用一个简单地asp.net core控制台程序来进行。

### 开始前的准备

1. 在我们的项目文件夹，单击鼠标右键选择“在当前文件夹下面打开Git Bash”

2. 然后输入`git checkout Master` 切换回Mater分支，然后输入`git checkout -b Sample05` 创建一个新的名为“Sample05”的分支，如下所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-84f1e80843263f5b.png?imageMogr2/auto-orient/strip|imageView2/2/w/745/format/webp)

   1543242325029

3. 使用vs2017创建一个新的项目，名称为“Sample05” 位置位于我们当前的目录，如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-0432063bc7b506c6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1178/format/webp)

   1543242490572

4. 接下来打开数据库，新建一个Content内容表，表结构还沿用之前教程中的实体，这里只给出MSSql的脚本：至于MySql的你自己建了，如果你实在不会的话可以到群里问其他小伙伴要吧

   ```mssql
   CREATE TABLE [dbo].[content](
    [id] [int] IDENTITY(1,1) NOT NULL,
    [title] [nvarchar](50) NOT NULL,
    [content] [nvarchar](max) NOT NULL,
    [status] [int] NOT NULL,
    [add_time] [datetime] NOT NULL,
    [modify_time] [datetime] NULL,
    CONSTRAINT [PK_Content] PRIMARY KEY CLUSTERED 
   (
    [id] ASC
   )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
   ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
   
   GO
   
   ALTER TABLE [dbo].[content] ADD  CONSTRAINT [DF_Content_status]  DEFAULT ((1)) FOR [status]
   GO
   
   ALTER TABLE [dbo].[content] ADD  CONSTRAINT [DF_content_add_time]  DEFAULT (getdate()) FOR [add_time]
   GO
   
   CREATE TABLE [dbo].[comment](
    [id] [int] IDENTITY(1,1) NOT NULL,
    [content_id] [int] NOT NULL,
    [content] [nvarchar](512) NOT NULL,
    [add_time] [datetime] NOT NULL,
    CONSTRAINT [PK_comment] PRIMARY KEY CLUSTERED 
   (
    [id] ASC
   )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
   ) ON [PRIMARY]
   
   GO
   
   ALTER TABLE [dbo].[comment] ADD  CONSTRAINT [DF_comment_add_time]  DEFAULT (getdate()) FOR [add_time]
   GO
   ```

5. 项目中新增数据库表对应的实体对象，代码如下：

   ```c
       public class Content
       {
           /// <summary>
           /// 主键
           /// </summary>
           public int id { get; set; }
   
           /// <summary>
           /// 标题
           /// </summary>
           public string title { get; set; }
           /// <summary>
           /// 内容
           /// </summary>
           public string content { get; set; }
           /// <summary>
           /// 状态 1正常 0删除
           /// </summary>
           public int status { get; set; }
           /// <summary>
           /// 创建时间
           /// </summary>
           public DateTime add_time { get; set; } = DateTime.Now;
           /// <summary>
           /// 修改时间
           /// </summary>
           public DateTime? modify_time { get; set; }
       }
   public class Comment
       {
           /// <summary>
           /// 主键
           /// </summary>
           public int id { get; set; }
           /// <summary>
           /// 文章id
           /// </summary>
           public int content_id { get; set; }
           /// <summary>
           /// 评论内容
           /// </summary>
           public string content { get; set; }
           /// <summary>
           /// 添加时间
           /// </summary>
           public DateTime add_time { get; set; } = DateTime.Now;
       }
   ```

6. 项目中添加Dapper的Nugets包，相信一路看教程过来的你一定知道怎么新增Nuget包吧，这里就不过多介绍了。

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-6c8a1dc7566349d3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1169/format/webp)

   1543243792492

### 实战演示

1. 插入操作：将一个对象插入到数据库中，代码如下：

   ```c
    /// <summary>
           /// 测试插入单条数据
           /// </summary>
          static void test_insert()
           {
               var content = new Content
               {
                   title = "标题1",
                   content = "内容1",
   
               };
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"INSERT INTO [Content]
                   (title, [content], status, add_time, modify_time)
   VALUES   (@title,@content,@status,@add_time,@modify_time)";
                   var result = conn.Execute(sql_insert, content);
                   Console.WriteLine($"test_insert：插入了{result}条数据！");
               }
           }
   ```

2. 一次批量插入多条数据，测试代码如下：

   ```c
   /// <summary>
           /// 测试一次批量插入两条数据
           /// </summary>
          static void test_mult_insert()
           {
               List<Content> contents = new List<Content>() {
                  new Content
               {
                   title = "批量插入标题1",
                   content = "批量插入内容1",
   
               },
                  new Content
               {
                   title = "批量插入标题2",
                   content = "批量插入内容2",
   
               },
           };
   
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"INSERT INTO [Content]
                   (title, [content], status, add_time, modify_time)
   VALUES   (@title,@content,@status,@add_time,@modify_time)";
                   var result = conn.Execute(sql_insert, contents);
                   Console.WriteLine($"test_mult_insert：插入了{result}条数据！");
               }
           }
   ```

3. 执行下代码查看到控制台输出如下的结果：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-dba239892ba810c2.png?imageMogr2/auto-orient/strip|imageView2/2/w/375/format/webp)

   1543246862147

   然后到数据库查看下表中的数据如下：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-f1daf200d2b00792.png?imageMogr2/auto-orient/strip|imageView2/2/w/558/format/webp)

   1543246898729

4. 下面我们再分别测试下删除一条数据，与一次删除多条数据吧，代码如下：

   ```c
    /// <summary>
           /// 测试删除单条数据
           /// </summary>
           static void test_del()
           {
               var content = new Content
               {
                   id = 2,
   
               };
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"DELETE FROM [Content]
   WHERE   (id = @id)";
                   var result = conn.Execute(sql_insert, content);
                   Console.WriteLine($"test_del：删除了{result}条数据！");
               }
           }
   
           /// <summary>
           /// 测试一次批量删除两条数据
           /// </summary>
           static void test_mult_del()
           {
               List<Content> contents = new List<Content>() {
                  new Content
               {
                   id=3,
   
               },
                  new Content
               {
                   id=4,
   
               },
           };
   
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"DELETE FROM [Content]
   WHERE   (id = @id)";
                   var result = conn.Execute(sql_insert, contents);
                   Console.WriteLine($"test_mult_del：删除了{result}条数据！");
               }
           }
   ```

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-caa56db4a78d540f.png?imageMogr2/auto-orient/strip|imageView2/2/w/328/format/webp)

   1543247418059

   然后去数据库里查看，发现主键为2，3，4的数据都已经被删除了，如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-eebfb9783d8c1fb1.png?imageMogr2/auto-orient/strip|imageView2/2/w/595/format/webp)

   1543247491525

5. 下面我们再测试下修改吧，也是分别测试一次只修改一条数据（主键为5），与一次批量修改多条数据（主键为6，7）

   ```c
   /// <summary>
           /// 测试修改单条数据
           /// </summary>
           static void test_update()
           {
               var content = new Content
               {
                   id = 5,
                   title = "标题5",
                   content = "内容5",
   
               };
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"UPDATE  [Content]
   SET         title = @title, [content] = @content, modify_time = GETDATE()
   WHERE   (id = @id)";
                   var result = conn.Execute(sql_insert, content);
                   Console.WriteLine($"test_update：修改了{result}条数据！");
               }
           }
   
           /// <summary>
           /// 测试一次批量修改多条数据
           /// </summary>
           static void test_mult_update()
           {
               List<Content> contents = new List<Content>() {
                  new Content
               {
                   id=6,
                   title = "批量修改标题6",
                   content = "批量修改内容6",
   
               },
                  new Content
               {
                   id =7,
                   title = "批量修改标题7",
                   content = "批量修改内容7",
   
               },
           };
   
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"UPDATE  [Content]
   SET         title = @title, [content] = @content, modify_time = GETDATE()
   WHERE   (id = @id)";
                   var result = conn.Execute(sql_insert, contents);
                   Console.WriteLine($"test_mult_update：修改了{result}条数据！");
               }
           }
   ```

   现在我们执行下测试代码看下结果吧

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-c09cd2ee2ad84d1c.png?imageMogr2/auto-orient/strip|imageView2/2/w/360/format/webp)

   1543248037237

   再到数据库中查看下数据，上步骤5中最后一张图相比较

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-d95a41daa02a92a8.png?imageMogr2/auto-orient/strip|imageView2/2/w/668/format/webp)

   1543248094960

6. 增删改都测试了，下面就开始测试查询吧，我们分别来测试下查询指定的数据以及一次查询多条数据来看下结果吧。还是先上代码，：

   ```c
    /// <summary>
           /// 查询单条指定的数据
           /// </summary>
           static void test_select_one()
           {
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"select * from [dbo].[content] where id=@id";
                   var result = conn.QueryFirstOrDefault<Content>(sql_insert, new { id=5});
                   Console.WriteLine($"test_select_one：查到的数据为：");
               }
           }
   
           /// <summary>
           /// 查询多条指定的数据
           /// </summary>
           static void test_select_list()
           {
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"select * from [dbo].[content] where id in @ids";
                   var result = conn.Query<Content>(sql_insert, new { ids=new int[] { 6,7} });
                   Console.WriteLine($"test_select_one：查到的数据为：");
               }
           }
   ```

   然后我们打上断点然后去看下结果吧！这里图片我没有截成功，所以就不贴了。

7. 关联查询，Dapper的强大之处就在于其关联查询了！为了测试的方便，我们给主键为5的content添加两个comment中，这个插入的代码就不贴出来了，留给大家自行书写吧，如果不会的话可以加群问群里的其他小伙伴吧。这里需要新建一个类

   ```c
   public class ContentWithCommnet
       {
           /// <summary>
           /// 主键
           /// </summary>
           public int id { get; set; }
   
           /// <summary>
           /// 标题
           /// </summary>
           public string title { get; set; }
           /// <summary>
           /// 内容
           /// </summary>
           public string content { get; set; }
           /// <summary>
           /// 状态 1正常 0删除
           /// </summary>
           public int status { get; set; }
           /// <summary>
           /// 创建时间
           /// </summary>
           public DateTime add_time { get; set; } = DateTime.Now;
           /// <summary>
           /// 修改时间
           /// </summary>
           public DateTime? modify_time { get; set; }
           /// <summary>
           /// 文章评论
           /// </summary>
           public IEnumerable<Comment> comments { get; set; }
       }
   ```

   然后就是测试代码，运行的查询测试代码如下：查询id为5的文章，文章是包含评论列表的

   代码如下：

   ```c
   static void test_select_content_with_comment()
           {
               using (var conn = new SqlConnection("Data Source=127.0.0.1;User ID=sa;Password=1;Initial Catalog=Czar.Cms;Pooling=true;Max Pool Size=100;"))
               {
                   string sql_insert = @"select * from content where id=@id;
   select * from comment where content_id=@id;";
                   using (var result = conn.QueryMultiple(sql_insert, new { id = 5 }))
                   {
                       var content = result.ReadFirstOrDefault<ContentWithComment>();
                       content.comments = result.Read<Comment>();
                       Console.WriteLine($"test_select_content_with_comment:内容5的评论数量{content.comments.Count()}");
                   }
   
               }
           }
   ```

   结果如下所示，调试的代码没法截图我也很无奈。

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-db184a821a86ddde.png?imageMogr2/auto-orient/strip|imageView2/2/w/488/format/webp)

   1543251360510

## GitHub源码

GitHub的测试源码已经上传，https://github.com/yilezhu/Czar.Cms/tree/Sample05 放在Czar.Cms的Sample05分支上面。大家可以参考下，觉得有用的话记得star哦！

## 总结

本文给大家演示了Dapper的常用方法，不过都是通过同步的方式进行操作的，如果你想使用异步的话可以自行进行测试。文中的大部分内容都有截图，个别调试无法截图的大伙可以自行调试查看！相信通过本文的实例讲解，大伙应该能够使用dapper进行相应的开发！下一篇文章我们将进行vue的讲解！当然也只是进行很浅层次的讲解。因为我是一个后端，也是抱着学习的态度来进行vue的记录的！主要是以快速上为主。