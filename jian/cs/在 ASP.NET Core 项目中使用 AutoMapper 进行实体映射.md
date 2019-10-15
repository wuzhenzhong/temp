# 在 ASP.NET Core 项目中使用 AutoMapper 进行实体映射

### 前言

​        在实际项目开发过程中，我们使用到的各种 ORM 组件都可以很便捷的将我们获取到的数据绑定到对应的 `List<T>` 集合中，因为我们最终想要在页面上展示的数据与数据库实体类之间可能存在很大的差异，所以这里更常见的方法是去创建一些对应于页面数据展示的 `视图模型` 类，通过对获取到的数据进行二次加工，从而满足实际页面显示的需要。

​        因此，如何更便捷的去实现 `数据库持久化对象` 与 `视图对象` 间的实体映射，避免我们在代码中去一次次的手工实现这一过程，就可以降低开发的工作量，而 AutoMapper 则是可以帮助我们便捷的实现实体转换这一过程的利器。所以，本章我们就来学习如何在 ASP.NET Core 项目中通过使用 AutoMapper 去完成实体间的映射。

​        当然，如果你习惯于从视图展现到持久化到数据库都采用数据库实体，那么本篇文章对你可能不会有任何的帮助。

​        仓储地址：[github.com/Lanesra712/…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FLanesra712%2Fgrapefruit-common%2Ftree%2Fmaster%2Fsample%2Faspnetcore%2Faspnetcore-automapper-tutorial)

### Step by Step

​        AutoMapper 是一个 OOM(Object-Object-Mapping) 组件，从名字上就可以看出来，这一系列的组件主要是为了帮助我们实现实体间的相互转换，从而避免我们每次都采用手工编写代码的方式进行转换。在没有采用 OOM 组件之前，如果我们需要实现类似于一份数据在不同客户端显示不同的字段，我们只能以手工的、逐个属性赋值的方式实现数据在各个客户端数据类型间的数据传递，而 OOM 组件则可以很方便的帮我们实现这一需求。

​        **一、几个概念**

​        在上面我们有提到 `数据库持久化对象` 和 `视图对象` 这两个概念，其实除了这两个对象的概念之外，还存在一个 `数据传输对象` 的概念，这里我们来简单阐述下这三种对象的概念。

​        `数据库持久化对象（Persistent Object）`：顾名思义，这个对象是用来将我们的数据持久化到数据库，一般来说，持久化对象中的字段会与数据库中对应的 table 保持一致。

​        这里，如果你采用了 DDD 的思想去指导设计系统架构，其实最终落地到我们代码中的其实是 `领域对象（Domain Object）`，它与 `数据库持久化对象` 最显著的差异在于 `领域对象` 会包含当前业务领域的各种事件，而 `数据库持久化对象` 仅是包含了数据库中对应 table 的数据字段信息。

​        `视图对象（View Object）`：视图对象 VO 是面向前端用户页面的，一般会包含呈现给用户的某个页面/组件中所包含的所有数据字段信息。

​        `数据传输对象（Data Transfer Object）`：数据传输对象 DTO 一般用于前端展示层与后台服务层之间的数据传递，以一种媒介的形式完成 `数据库持久化对象` 与 `视图对象` 之间的数据传递。

​        这里通过一个简单的示意图去解释下这三种对象的具体使用场景，在这个示例的项目中，我省略了数据传输对象，将数据库持久化对象直接转换成页面显示的视图对象。

![数据模型流转示意](https://user-gold-cdn.xitu.io/2019/10/6/16da142927f5f0c8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        **二、组件加载**

​        首先我们需要通过 Nuget 将 AutoMapper 加载到项目中，因为这个示例项目只包含一个 MVC 的项目，并没有多余的分层，所以这里需要将两个使用到的 dll 都添加到这个 MVC 项目中。

```
Install-Package AutoMapper
Install-Package AutoMapper.Extensions.Microsoft.DependencyInjection
复制代码
```



![组件加载](https://user-gold-cdn.xitu.io/2019/10/6/16da142c8ebb9f22?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        这里我添加了 `AutoMapper.Extensions.Microsoft.DependencyInjection` 这个程序集，从这个程序集的名字就可以看出来，这个程序集主要是为了我们可以通过依赖注入的方式在项目中去使用 AutoMapper。

​        在 .NET Fx 的时代，我们使用 AutoMapper 时，可能就像下面的代码一样，更多的是通过 `Mapper` 的几个静态方法来实现实体间的映射，不过在 .NET Core 程序中，我们首选还是采用依赖注入的方式去完成实体间的映射。

```
// 构建实体映射规则
Mapper.Initialize(cfg => cfg.CreateMap<OrderModel, OrderDto>());

// 实体映射
var order = new OrderModel{};
OrderDto dto = Mapper.Map<OrderModel,OrderDto>(order);
复制代码
```

​        **三、使用案例**

​        因为原本想要使用的示例项目是之前的 [ingos-server](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FLanesra712%2Fingos-server%5D) 这个项目，由于目前自己有在学习 DDD 的知识，并且有在按照微软的 [eShopOnContainers](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fdotnet-architecture%2FeShopOnContainers) 这个项目中基于 DDD 思想设计的框架，对自己的这个 ingos-server 项目进行 DDD 化的调整，嗯，其实就是照葫芦画瓢，所以目前整个项目被我改的乱七八糟的，不太适合作为示例项目了，所以这里新创建了一个比较单纯的 ASP.NET Core MVC 项目来作为这篇文章的演示项目。

​        因为这个示例项目只是为了演示如何在 ASP.NET Core 项目中去使用 AutoMapper，所以这里并没有进行分层，整个示例页面的运行流程就是，PostController 中的 List Action 调用 PostAppService 类中的 GetPostLists 方法去获取所有的文章数据，同时在这个方法中会进行实体映射，将我们从 PostDomain 中获取到的 PO 对象转换成页面展示的 VO 对象，项目中每个文件夹的作用见下图所示。

![示例代码说明](https://user-gold-cdn.xitu.io/2019/10/6/16da143204b13612?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        这里的示例项目是演示当我们从数据库获取到需要的数据后，如何完成从 PO 到 VO 的实体映射，PostModel（PO）和 PostViewModel（VO）的类定义如下所示。

```
public class PostModel
{
    public Guid Id { get; set; }
    public long SerialNo { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
    public string Image { get; set; }
    public short CategoryCode { get; set; }
    public bool IsDraft { get; set; }
    public string Content { get; set; }
    public DateTime ReleaseDate { get; set; }
    public virtual IList<CommentModel> Comments { get; set; }
}

public class PostViewModel
{
    public Guid Id { get; set; }
    public long SerialNo { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
    public short CategoryCode { get; set; }
    public string Category => CategoryCode == 1001 ? ".NET" : "杂谈";
    public string ReleaseDate { get; set; }
    public short CommentCounts { get; set; }
    public virtual int Count { get; set; }
}
复制代码
```

​        首先我们需要创建一个实体映射的配置类，需要继承于 AutoMapper 的 Profile 类，在无参构造函数中，我们就可以通过 CreateMap 方法去创建两个实体间的映射关系。

```
public class PostProfile : Profile
{
    /// <summary>
    /// ctor
    /// </summary>
    public PostProfile()
    {
        // 配置 mapping 规则
        //
        CreateMap<PostModel, PostViewModel>();
    }
}
复制代码
```



![配置映射关系](https://user-gold-cdn.xitu.io/2019/10/6/16da1435e68d5cb6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        通过泛型的 CreateMap 方法就可以完成我们从 PostModel(PO) 到 PostViewModel(VO) 的实体映射。当然，因为 AutoMapper 默认是通过匹配字段名称和类型进行自动匹配，所以如果你进行转换的两个类的中的某些字段名称不一样，这里我们就需要进行手动的编写转换规则。

​        就像在这个需要进行实体映射的示例代码中，PostViewModel 中的 CommentCounts 字段是根据 PostModel 中 CommentModel 集合的数据个数进行赋值的，所以这里我们就需要对这个字段的转换规则进行修改。

​        在 AutoMapper 中，我们可以通过 ForMember 方法对映射规则做进一步的加工。这里我们需要指明 PostViewModel 的 CommentCounts 字段的值是通过对 PostModel 中的 Comments 信息进行求和从而获取到的，最终实现的转换代码如下所示。

```
public class PostProfile : Profile
{
    /// <summary>
    /// ctor
    /// </summary>
    public PostProfile()
    {
        // 配置 mapping 规则
        //
        CreateMap<PostModel, PostViewModel>()
            .ForMember(destination => destination.CommentCounts, source => source.MapFrom(i => i.Comments.Count()));
    }
}
复制代码
```

​        ForMember 方法不仅可以进行指定不同名称的字段进行转换，也可以通过编写规则实现字段类型的转换。例如这里 PO 中的 ReleaseDate 字段其实是 DateTime 类型的，我们需要通过编写规则将该字段对应到 VO 中 string 类型的 ReleaseDate 字段上，最终的实现代码如下所示。

```
public class PostProfile : Profile
{
    /// <summary>
    /// ctor
    /// </summary>
    public PostProfile()
    {
        // Config mapping rules
        //
        CreateMap<PostModel, PostViewModel>()
            .ForMember(destination => destination.CommentCounts, source => source.MapFrom(i => i.Comments.Count()))
            .ForMember(destination => destination.ReleaseDate, source => source.ConvertUsing(new DateTimeConverter()));
    }
}

public class DateTimeConverter : IValueConverter<DateTime, string>
{
    public string Convert(DateTime source, ResolutionContext context)
        => source.ToString("yyyy-MM-dd HH:mm:ss");
}
复制代码
```

​        这里很多人可能习惯将所有的实体映射规则都放到同一个 Profile 文件里面，因为这里采用是单体架构的项目，所以整个项目中会存在不同的模块，所以这里我是按照每个模块去创建对应的 Profile 文件。实际在 ingos-server 这个项目中的使用方式见下图所示。

![实际使用](https://user-gold-cdn.xitu.io/2019/10/6/16da143ba2274a92?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        当我们创建好对应的映射规则后，因为我们是采用依赖注入的方式进行使用，所以这里我们就需要将我们的匹配规则注入到 IServiceCollection 中。从之前加载的程序集的 [github readme](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FAutoMapper%2FAutoMapper.Extensions.Microsoft.DependencyInjection) 描述中可以看到，我们需要将配置好的 Profile 类通过 AddAutoMapper 这个扩展方法进行注入。

​        因为我们在实际项目中可能存在多个自定义的 Profile 文件，而我们肯定是需要将这些自定义规则都注入到 IServiceCollection 中。所以我在 AddAutoMapper 这个方法的基础上创建了一个 AddAutoMapperProfiles 方法去注入我们的实体映射规则。

​        通过 AutoMapper 的说明我们可以看出来，所有的自定义的 Profile 类都是需要继承于 AutoMapper 的 Profile 基类，所以这里我是采用反射的方式，通过获取到程序集中所有继承于 Profile 类的类文件进行批量的注入到 IServiceCollection 中，具体的实现代码如下所示。

```
/// <summary>
/// Automapper 映射规则配置扩展方法
/// </summary>
public static class AutoMapperExtension
{
    public static IServiceCollection AddAutoMapperProfiles(this IServiceCollection services)
    {
        // 从 appsettings.json 中获取包含配置规则的程序集信息
        string assemblies = ConfigurationManager.GetConfig("Assembly:Mapper");

        if (!string.IsNullOrEmpty(assemblies))
        {
            var profiles = new List<Type>();

            // 获取继承的 Profile 类型信息
            var parentType = typeof(Profile);

            foreach (var item in assemblies.Split(new char[] { '|' }, StringSplitOptions.RemoveEmptyEntries))
            {
                // 获取所有继承于 Profile 的类
                //
                var types = Assembly.Load(item).GetTypes()
                    .Where(i => i.BaseType != null && i.BaseType.Name == parentType.Name);

                if (types.Count() != 0 || types.Any())
                    profiles.AddRange(types);
            }

            // 添加映射规则
            if (profiles.Count() != 0 || profiles.Any())
                services.AddAutoMapper(profiles.ToArray());
        }

        return services;
    }
}
复制代码
```

​        因为我是将需要加载的程序集信息放到配置文件中的，所以这里我们只需要将包含 Profile 规则的程序集添加到对应的配置项下面就可以了，此时如果包含多个程序集，则需要使用 `|` 进行分隔。

```
{
  "Assembly": {
    "Mapper": "aspnetcore-automapper-tutorial"
  }
}
复制代码
```

​        当我们将所有的实体映射规则注入到 IServiceCollection 中，就可以在代码中使用这些实体映射规则。和其它通过依赖注入的接口使用方式相同，我们只需要在使用到的地方注入 IMapper 接口，然后通过 Map 方法就可以完成实体间的映射，使用的代码如下。

```
public class PostAppService : IPostAppService
{
    #region Initialize

    /// <summary>
    ///
    /// </summary>
    private readonly IPostDomain _post;

    /// <summary>
    ///
    /// </summary>
    private readonly IMapper _mapper;

    /// <summary>
    /// ctor
    /// </summary>
    /// <param name="post"></param>
    /// <param name="mapper"></param>
    public PostAppService(IPostDomain post, IMapper mapper)
    {
        _post = post ?? throw new ArgumentNullException(nameof(post));
        _mapper = mapper ?? throw new ArgumentNullException(nameof(mapper));
    }

    #endregion Initialize

    /// <summary>
    /// 获取所有的文章信息
    /// </summary>
    /// <returns></returns>
    public IList<PostViewModel> GetPostLists()
    {
        var datas = _post.GetPostLists();
        return _mapper.Map<IList<PostModel>, IList<PostViewModel>>(datas);
    }
}
复制代码
```

​        至此我们就实现了在 ASP.NET Core 项目中使用 AutoMapper，实现后的结果如下图所示。

![示意图](https://user-gold-cdn.xitu.io/2019/10/6/16da1441784fa96c?imageslim)



### 总结

​        本篇文章主要是演示下如何在 ASP.NET Core 项目中去使用 AutoMapper 来实现实体间的映射，因为之前只是在 .NET Fx 项目中有使用过这个组件，并没有在 .NET Core 项目中使用，所以这次趁着国庆节假期就来尝试如何在 .NET Core 项目中使用，整个组件使用起来其实是很简单的，但是使用后却可以给我们在实际的项目开发中省很多的事，所以就把自己的使用方法分享出来，如果对你有些许的帮助的话，不胜荣幸~~~

> ### 占坑
>
> ​        作者：[墨墨墨墨小宇](https://juejin.im/user/5bd93a936fb9a0224268c11b)
> ​        个人简介：96年生人，出生于安徽某四线城市，毕业于Top 10000000 院校。.NET程序员，枪手死忠，喵星人。于2016年12月开始.NET程序员生涯，微软.NET技术的坚定坚持者，立志成为云养猫的少年中面向谷歌编程最厉害的.NET程序员。
> ​        个人博客：[yuiter.com](https://link.juejin.im/?target=https%3A%2F%2Fyuiter.com)
> ​        博客园博客：[www.cnblogs.com/danvic712](https://link.juejin.im/?target=https%3A%2F%2Fwww.cnblogs.com%2Fdanvic712)

关注下面的标签，发现更多相似文章