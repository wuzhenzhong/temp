# .net core 无处不在的依赖注入

## `.net core` 已经自带了依赖注入，使用非常方便

`.net core` 中`DI`的核心

- `IServiceCollection` 负责注册
- `IServiceProvider` 负责提供实例

### IServiceCollection

提供三个不同生命周期的实例注入方法

- ```
  AddTransient
  ```

  - 每一次`GetService`都会创建一个新的实例

- ```
  AddSingleton
  ```

  - 整个应用程序生命周期只创建一个实例（单例）

- ```
  AddScoped
  ```

  - 在同一个`Scope`内只初始化一个实例 （可以理解为每一个线程一个实例 `http request`）,比如使用`EF`的`DbContext`，在一个请求中可能跨越其他的`Repository`共享一个事物

对应了`Microsoft.Extensions.DependencyInjection.ServiceLifetime`的三个枚举值

```
public enum ServiceLifetime
{
  Singleton,
  Scoped,
  Transient
}
复制代码
```

`.net core`已经为我们默认注入了`IServiceCollection`,在`.net core web`项目的`startup`的`ConfigureServices`中尽情使用

注入一个`EF DbContext`上下文对象

```
services.AddDbContextPool<UserDbContext>(options => options.UseMySQL(userDbConnstr));
复制代码
```

#### 注入自定义服务

```
services.AddScoped<IWebClient, MyWebClient>();
services.AddScoped(typeof(MyWebServiceProxy));  //自注入
复制代码
```

`Controller`使用

```
private UserDbContext _dbContext { get; set; }
public UsersController(UserDbContext dbContext){
    _dbContext = dbContext;
}
复制代码
```

### 在`startup`外部注入服务

我们在`ConfigureServices`看到 `services.AddMvc()`这样的方法，都是通过`IServiceCollection`注入进去的，所以我们直接写扩展方法就好了

```
public static classs ConfigurationHelper{
    public static IServiceCollection ConfigMyServices(this IServiceCollection service){
        //注入你自定义的服务
        service.AddTransient<,>();
        service.AddScoprd<,>();
        return service;
    }
}

//startup中可以直接使用了
services.ConfigMyServices();
复制代码
```

### 如何拿到注入的服务

```
.Net Core`默认注入是构造函数方式，如上的`UserDbContext`，如果我们要想在任意地方拿到注入的服务，需要使用`IServiceProvider
```

可以通过`.net core`为我们提供的`IServiceCollection`的扩展方法`BuildServiceProvider()`,构建一个`IServiceProvider`实例

`ServiceProvider`提供的`GetService`方法，可以让我们随时取注入的实例

综上，我们可以将`IServiceProvider`赋值给一个全局的对象，这样就可以随处使用了

```
public class GlobalAppConst{
    public static IServiceProvider ServiceProvider{get;set;}
}

//在startup全部注入完毕后，构建ServiceProvider对象
GlobalAppConst.ServiceProvider = services.BuildServiceProvider();

//其他类中使用
GlobalAppConst.ServiceProvider.GetService(typeof(UserDbContext)) as UserDbContext;
复制代码
```

#### 通过`HttpContext`上下文来获取实例

```
HttpContext.RequestServices.GetService<,>();
复制代码
```

### 如何替换为其他的Ioc容器

`Autofac`提供的泛型注入

```
ioc.RegisterGeneric(typeof(BaseAppService<>))
    .As(typeof(IBaseAppService<>))
    .AsImplemenedInterfaces()
    .InstancePerLifetimeScope();
复制代码
```

如果要替换`Ioc`容器，只需要把`startup`的`ConfigureService`方法的返回值从`void`改为`IServiceProvider`即可

```
//Add Autofac Ioc
var containerBuilder = new ContainerBuilder();
containerBuilder.Populate(services);
// ...在这里使用builder对象进行注入
var container = containerBuilder.Build();
return new AutofacServiceProvider(container);
复制代码
```

替换`Ioc`容器的变化就是整个`Request`生命周期被`.net core`管理了，所以`Autofac`将不再有效，可以使用`InstancePerLifetimeScope`同样是有用的，对应`.net core`中的`Scoped`

#### 实用案例：系统启动按规则自动注入某些模块

```
//1.首先加载所有依赖的`dll`文件
var types = Directory.EnuerateFiles(path,"*.dll")
                .Select(Assembly.LoadFrom)
                .SelectMany(c=>c.GetTypes())
                .ToList();

//2.自动注入实现了IDependency接口的子类

var autoInjectTypes = types.Where(c=>!c.IsAbstract &&
                         typeof(IDependency).IsAssignableFrom(c))
                        .ToList();
                        
foreach(type in autoInjectTypes){
    services.AddSingleton(typeof(IDependency),type)
};

//build....
复制代码
```

#### 系统启动按名称自动注入某些模块，带泛型注入

类名以`AppService`结尾的服务，都自动注入起来

**.net core 提供的注入**

```
var types = new List<Type>();
var allAssemblys = Assembly.GetEntryAssembly().GetReferencedAssemblies().Select(Assembly.Load).Where(c => c.FullName.StartsWith("EF.Core.Demo")).ToList(); 
// EF.Core.Demo 程序集名称
foreach (var assembly in allAssemblys)
{
    foreach (var assemblyDefineType in assembly.DefinedTypes)
    {
        if (assemblyDefineType.FullName.EndsWith("AppService"))
        {
            types.Add(assemblyDefineType.AsType());
        }
    }
}
var implementTypes = types.Where(t => t.IsClass).ToList();
foreach (var implementType in implementTypes)
{
    //接口和实现的命名规则为："UserAppService"类实现了"IUserAppService"接口,你也可以自定义规则
    var interfaceType = implementType.GetInterface("I" + implementType.Name);
    if (interfaceType != null)
    {
        services.AddScoped(interfaceType, implementType);
    }
}
// 泛型注入
services.AddScoped(typeof(IBaseAppService<>), typeof(BaseAppService<>));
复制代码
```

**使用Autofac替换.net core内部的注入**

```
Nuget`安装`Autofac、Autofac.AspNetCore.Multitenant
var containerBuilder = new ContainerBuilder();
containerBuilder.Populate(services);

containerBuilder.RegisterAssemblyTypes(Assembly.GetEntryAssembly().GetReferencedAssemblies().Select(Assembly.Load).ToArray())
    .Where(t => t.Name.EndsWith("AppService") && !t.IsAbstract)
    .AsImplementedInterfaces()
    .InstancePerLifetimeScope();
containerBuilder.RegisterGeneric(typeof(BaseAppService<>)).As(typeof(IBaseAppService<>)).AsImplementedInterfaces().InstancePerLifetimeScope();
var container = containerBuilder.Build();
return new AutofacServiceProvider(container);

// ConfigureServices 方法的返回值由void改为IServiceProvider
复制代码
```

**服务和实现**

```
public interface IBaseAppService<TEntity>
    where TEntity : class, new()
{
    TEntity Find(string id);
}

public class BaseAppService<TEntity> : IBaseAppService<TEntity>
    where TEntity : class,new()
{
    private readonly UserDbContext _dbContext;
    public BaseAppService(UserDbContext dbContext)
    {
        _dbContext = dbContext;
    }

    public TEntity Find(string id)
    {
        return _dbContext.Set<TEntity>().Find(id);
    }
}
复制代码
```

**控制器注入使用**

```
[Route("api/[controller]")]
[ApiController]
public class UserController : ControllerBase
{
    private readonly UserDbContext _dbContext;
    private readonly IUserAppService _userService;

    private readonly IBaseAppService<User> _userAppService;

    public UserController(UserDbContext dbContext, IUserAppService userService, IBaseAppService<User> userAppService)
    {
        _dbContext = dbContext;
        _userService = userService;
        _userAppService = userAppService;
    }

    [HttpGet("name")]
    public string GetName()
    {
        return _userService.GetName();
    }

    [HttpGet("{id}")]
    public User GetById(string id)
    {
        //return _dbContext.Users.Find(id);
        return _userAppService.Find(id);
    }
}
复制代码
```

### 系统默认给我们注册的常用服务

`core`取消了一些常用的静态类型，如`HttpContext`,`ConfigurationManager`等，取而代之的是帮我们默认注册了一些服务

- `IHostingEnvironment` 包含了环境名称、应用名称以及当前程序所处的根目录
- `IHttpContextAccessor` 访问当前请求的`HttpContext`
- `IConfiguration` 配置对象 `serviceProvider.GetService<IConfiguration>() as IConfiguration;`
- `IServiceProvider` 服务提供注入实例
- `DbContext` `EFCore`的`DbContext`