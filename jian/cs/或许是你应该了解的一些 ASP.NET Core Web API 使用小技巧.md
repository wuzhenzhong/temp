# 或许是你应该了解的一些 ASP.NET Core Web API 使用小技巧

### 前言

​        在目前的软件开发的潮流中，不管是前后端分离还是服务化改造，后端更多的是通过构建 API 接口服务从而为 web、app、desktop 等各种客户端提供业务支持，如何构建一个符合规范、容易理解的 API 接口是我们后端开发人员需要考虑的。在本篇文章中，我将列举一些我在使用 ASP.NET Core Web API 构建接口服务时使用到的一些小技巧，因才疏学浅，可能会存在不对的地方，欢迎指出。

​        仓储地址：[github.com/Lanesra712/…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FLanesra712%2Fingos-server)

### Step by Step

​        因为本篇文章中涉及到的一些知识点在之前的文章中也已经有具体的解释了，所以这里只会说明如何在 ASP.NET Core Web API 中如何去使用，不会做过多的详细介绍。如果你需要详细了解的话，可以跳转到文章中给出的外链地址去查看。

​        本篇文章中使用的代码是基于 .NET Core 2.2 + .NET Standard 2.0 进行构建的，如果你采用的版本与我使用的不同，可能最终实现起来的代码会有所不同，请提前知悉。同时，本篇文章中所有示例代码都会存在于前言中所列出的 github repo 中，我会尝试将每个功能点的开发作为一次 commit，并且也会在后续进行不定期的更新完善，最终搭建一个基于领域驱动思想的后端项目模板，如果对你有帮助的话，欢迎持续关注。

​        **一、使用小写路由**

​        在我之前的一篇文章中（[构建可读性更高的 ASP.NET Core 路由](https://juejin.im/post/5cf07eef518825189f6fa0cc)）有提到过，因为 .NET 默认采用 Pascal 的类命名方式，如果采用默认生成的路由，最终构建出的路由地址会存在大小写混在一起的情况，虽然在 .NET Core 中大小写的路由地址最终都会对于到正确的资源上，但是为了更好的符合前端的规范，所以这里我们首先按照之前的文章中所列出的方法去修改默认生成的路由地址格式。

​        因为这里我们最终想要实现的是符合 Restful 风格的 API 接口，所以这里我们首先需要将默认生成的 URL 地址改为全小写模式。

```
public void ConfigureServices(IServiceCollection services)
{
    // 采用小写的 URL 路由模式
    services.AddRouting(options =>
    {
        options.LowercaseUrls = true;
    });
}
复制代码
```



![小写路由](https://user-gold-cdn.xitu.io/2019/7/27/16c326e81573f71a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        如果你有看过[构建可读性更高的 ASP.NET Core 路由](https://juejin.im/post/5cf07eef518825189f6fa0cc)



​        如果你有查看 .NET Core 默认模板中生成的 API Controller，仔细看下，这里其实是使用的特性路由，所以这里我们并不能通过 Startup.UseMvc 定义的传统路由模板，或是直接在 Startup.Configure 中的 UseMvcWithDefaultRoute 方法去修改我们的生成的路由地址格式。

```
[Route("api/[controller]")]
[ApiController]
public class ValuesController : ControllerBase
{
}
复制代码
```

​        **二、允许跨域请求**

​        不管是后端接口的服务化改造，还是只是单纯的前后端分离项目开发，我们的前端项目与后端接口通常不会部署在一起，所以我们需要解决前端访问接口时会涉及到的跨域访问的问题。

​        针对跨域请求，我们可以采用 jsonp、或者是通过给 nginx 服务器配置响应的 header 参数头信息、或者是使用 CORS，又或是其它的解决方案。你可以自由选择，这里我采用在后端接口中直接配置对于 CORS 的支持。

​        在 .NET Core 中，已经在 Microsoft.AspNetCore.Cors 这个类库中添加了对于 CORS 的支持，因为这个类库是存在于我们已经安装的 .NET Core SDK 中，所以这里我们并不需要通过 Nuget 进行安装，可以直接使用。

​        在 .NET Core 中配置 CORS 规则，我们可以通过在 Startup.ConfigureServices 这个方法中添加不同的授权策略，之后再针对某个 Controller 或是 Action 通过添加 EnableCors 这个 Attribute 的方式进行配置，这里如果指定了 policy 策略名称，则会使用指定的策略，如果没有指定，则适用于系统的默认配置。同样的，我们也可以只设置一个策略，直接针对整个项目进行配置，这里我采用对整个项目采用通用的跨域请求配置方案。

​        在配置 CORS 策略时，我们可以设置只允许来源于某些 URL 地址的请求可以访问，或者是指定接口只允许某些 HTTP 方法进行访问，或者是在请求的 header 中必须包含某些信息才可以访问我们的接口。

​        在下面的代码中，我定义了针对整个项目的跨域请求策略，这里我只是设置了对于接口请求方 URL 地址的控制，通过读取配置文件中的数据，从而达到只允许某些 IP 可以访问的我们接口的目的。

```
public class Startup
{
    // 默认的跨域请求策略名称
    private const string _defaultCorsPolicyName = "Ingos.Api.Cors";

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc(
            // 添加 CORS 授权过滤器
            options => options.Filters.Add(new CorsAuthorizationFilterFactory(_defaultCorsPolicyName))
        ).SetCompatibilityVersion(CompatibilityVersion.Version_2_2);

        // 配置 CORS 授权策略
        services.AddCors(options => options.AddPolicy(_defaultCorsPolicyName,
            builder => builder.WithOrigins(
                    Configuration["Application:CorsOrigins"]
                    .Split(",", StringSplitOptions.RemoveEmptyEntries).ToArray()
                )
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        // 允许跨域请求访问
        app.UseCors(_defaultCorsPolicyName);
    }
}
复制代码
```

​        例如在下面的设置中，我只允许这一个地址可以访问我们的接口，如果需要指定多个的话，则可以通过英文的 , 进行分隔。

```
"Application": {
    "CorsOrigins": "http://127.0.0.1:5050"
}
复制代码
```

​        某些情况下，如果我们不想进行限制的话，只需要将值改为 * 即可。

```
"Application": {
    "CorsOrigins": "*"
}
复制代码
```

​        **三、添加接口版本控制**

​        在一些涉及到接口功能升级的场景下，当我们需要修改接口逻辑而旧版本的接口无法停用的情况时，为了减少对于原有接口的影响，我们可以采取为接口添加版本信息的形式，从而降低因采用不同版本而造成的影响。如果你想要详细了解的话，可以查看这篇文章，电梯直达 =》[ASP.NET Core 实战：构建带有版本控制的 API 接口](https://juejin.im/post/5c22e2cb5188257507558ddd)。

​        在实现具有版本控制的接口前，首先我们需要通过 Nuget 添加下面的两个 dll，因为我是在 Ingos.Api.Core 这个类库中进行配置的，所以我安装到了这个类库下，你需要根据你自己的情况选择最终是安装到 Api 接口项目中还是在别的类库下。

```
Install-Package Microsoft.AspNetCore.Mvc.Versioning
Install-Package Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer
复制代码
```

​        在安装完成之后，我们就可以在 Startup.ConfigureServices 方法中，为项目中的接口配置版本信息，这里我采用的方案是将版本号添加到接口的 URL 地址中。

​        因为对于所有中间件的配置都会在 Startup.ConfigureServices 方法中，为了保持该方法的纯净性，这里我写了一个扩展方法用于配置我们的 api 的版本，之后直接调用即可。

```
public static class ApiVersionExtension
{
    /// <summary>
    /// 添加 API 版本控制扩展方法
    /// </summary>
    /// <param name="services">生命周期中注入的服务集合 <see cref="IServiceCollection"/></param>
    public static void AddApiVersion(this IServiceCollection services)
    {
        // 添加 API 版本支持
	    services.AddApiVersioning(o =>
	    {
	        // 是否在响应的 header 信息中返回 API 版本信息
	        o.ReportApiVersions = true;
	
	        // 默认的 API 版本
	        o.DefaultApiVersion = new ApiVersion(1, 0);
	
	        // 未指定 API 版本时，设置 API 版本为默认的版本
	        o.AssumeDefaultVersionWhenUnspecified = true;
	    });

	    // 配置 API 版本信息
	    services.AddVersionedApiExplorer(option =>
	    {
	        // api 版本分组名称
	        option.GroupNameFormat = "'v'VVVV";
	
	        // 未指定 API 版本时，设置 API 版本为默认的版本
	        option.AssumeDefaultVersionWhenUnspecified = true;
	    });
    }
}
复制代码
```

​        扩展方法最终实现方式如上面的代码所示，之后我们就可以直接在 ConfigureServices 方法中直接进行调用这个扩展方法就可以了。

```
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    // Config api version
    services.AddApiVersion();
}
复制代码
```

​        现在我们删除项目创建时默认生成的 ValuesController，在 Controllers 目录下建立一个 v1 文件夹，代表此文件夹下都是 v1 版本的控制器。添加一个 UsersController 用来获取系统的用户资源，现在项目的文件结构如下图所示。

![控制器文件夹结构](https://user-gold-cdn.xitu.io/2019/7/27/16c32703cc48040c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        现在我们来改造我们的 UsersController，我们只需要在 Controller 或是 Action 上添加 ApiVersion 特性就可以指定当前 Controller/Action 的版本信息。同时，因为我需要将 API 的版本信息添加到生成的 URL 地址中，所以这里我们需要修改特性路由的模板，将我们的版本以占位符的形式添加到生成的路由 URL 地址中，修改完成后的代码及实现的效果如下所示。



```
[ApiVersion("1.0")]
[ApiController]
[Route("api/v{version:apiVersion}/[controller]")]
public class UsersController : ControllerBase
{
}
复制代码
```



![接口版本控制接口](https://user-gold-cdn.xitu.io/2019/7/27/16c3270b4162096b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        **四、添加对于 Swagger 接口文档的支持**

​        在前后端分离开发的情况下，我们需要提供给前端开发人员一个接口文档，从而让前端开发人员知道以什么样的 HTTP 方法或是传递什么样的参数给后端接口，从而获取到正确的数据，而 Swagger 则提供了一种自动生成接口文档的方式，同时也提供类似于 Postman 的功能，可以实现对于接口的实时调用测试。

​        首先，我们需要通过 Nuget 添加 Swashbuckle.AspNetCore 这个 dll 文件，之后我们就可以在此基础上实现对于 Swagger 的配置。

```
Install-Package Swashbuckle.AspNetCore
复制代码
```

​        与上面配置 API 接口的版本信息相似，这里我依旧采用构建扩展方法的方式来实现对于 Swagger 中间件的配置。具体的配置过程可以查看我之前写的文章（[ASP.NET Core 实战：构建带有版本控制的 API 接口](https://juejin.im/post/5c22e2cb5188257507558ddd)），这里只列出最终配置完成的代码。

```
public static void AddSwagger(this IServiceCollection services)
{
    // 配置 Swagger 文档信息
    services.AddSwaggerGen(s =>
    {
        // 根据 API 版本信息生成 API 文档
        //
        var provider = services.BuildServiceProvider().GetRequiredService<IApiVersionDescriptionProvider>();

        foreach (var description in provider.ApiVersionDescriptions)
        {
            s.SwaggerDoc(description.GroupName, new Info
            {
                Contact = new Contact
                {
                    Name = "Danvic Wang",
                    Email = "danvic96@hotmail.com",
                    Url = "https://yuiter.com"
                },
                Description = "Ingos.API 接口文档",
                Title = "Ingos.API",
                Version = description.ApiVersion.ToString()
            });
        }

        // 在 Swagger 文档显示的 API 地址中将版本信息参数替换为实际的版本号
        s.DocInclusionPredicate((version, apiDescription) =>
        {
            if (!version.Equals(apiDescription.GroupName))
                return false;

            var values = apiDescription.RelativePath
                .Split('/')
                .Select(v => v.Replace("v{version}", apiDescription.GroupName)); apiDescription.RelativePath = string.Join("/", values);
            return true;
        });

        // 参数使用驼峰命名方式
        s.DescribeAllParametersInCamelCase();

        // 取消 API 文档需要输入版本信息
        s.OperationFilter<RemoveVersionFromParameter>();

        // 获取接口文档描述信息
        var basePath = Path.GetDirectoryName(AppContext.BaseDirectory);
        var apiPath = Path.Combine(basePath, "Ingos.Api.xml");
        s.IncludeXmlComments(apiPath, true);
    });
}
复制代码
```

​        当我们配置完成后就可以在 Startup 类中去启用 Swagger 文档。

```
public void ConfigureServices(IServiceCollection services)
{
    // 添加对于 swagger 文档的支持
    services.AddSwagger();
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env, IApiVersionDescriptionProvider provider)
{
    // 启用 Swagger 文档
    app.UseSwagger();
    app.UseSwaggerUI(s =>
    {
        // 默认加载最新版本的 API 文档
        foreach (var description in provider.ApiVersionDescriptions.Reverse())
        {
            s.SwaggerEndpoint($"/swagger/{description.GroupName}/swagger.json",
                $"Sample API {description.GroupName.ToUpperInvariant()}");
        }
    });
}
复制代码
```



![Swagger 文档](https://user-gold-cdn.xitu.io/2019/7/27/16c327146f901da4?imageslim)

​        因为我们在之前设置构建的 API 路由时包含了版本信息，所以在最终生成的 Swagger 文档中进行测试时，我们都需要在参数列表中添加 API 版本这个参数。这无疑是有些不方便，所以这里我们可以通过继承 IOperationFilter 接口，控制在生成 API 文档时移除 API 版本参数，接口的实现方法如下所示。



```
public class RemoveVersionFromParameter : IOperationFilter
{
    public void Apply(Operation operation, OperationFilterContext context)
    {
        var versionParameter = operation.Parameters.Single(p => p.Name == "version");
        operation.Parameters.Remove(versionParameter);
    }
}
复制代码
```

​        当我们实现自定义的接口后就可以在之前针对 Swagger 的扩展方法中调用这个过滤方法，从而实现移除版本信息的目的，扩展方法中的添加位置如下所示。

```
public static void AddSwagger(this IServiceCollection services)
{
    // 配置 Swagger 文档信息
    services.AddSwaggerGen(s =>
    {
        // 取消 API 文档需要输入版本信息
        s.OperationFilter<RemoveVersionFromParameter>();
    });
}
复制代码
```

​        最终的实现效果如下图所示，可以看到，参数列表中已经没有版本信息这个参数，但是我们在进行接口测试时会自动帮我们添加上版本参数信息。

![移除版本参数](https://user-gold-cdn.xitu.io/2019/7/27/16c3271a564fbf16?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        这里需要注意，因为我们需要在最终生成的 Swagger 文档中显示出我们对于 Controller 或是 Action 添加的注释信息，所以这里我们需要在 Web Api 项目的属性选项中勾选上输出 XML 文档文件。同时如果你不想 VS 一直提示你有方法没有添加参数信息，这里我们可以在取消显示警告这里添加上 1591 这个参数。

![生成接口描述文件](https://user-gold-cdn.xitu.io/2019/7/27/16c3271dc6c8aa11?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



​        **五、构建符合 Restful 风格的接口**

​        在没有采用 Restful 风格来构建接口返回值时，我们可能会习惯于在接口返回的信息中添加一个接口是否请求成功的标识，就像下面代码中示例的这种返回形式。

```
{
	sueecss: true
	msg: '',
	data: [{
		id: '20190720214402',
		name: 'zhangsan'
	}]
}
复制代码
```

​        但是，当我们想要构建符合 Restful 风格的接口时，我们就不能再这样进行设计了，我们应该通过返回的 HTTP 响应状态码来标识这次访问是否成功。一些比较常用的 HTTP 状态码如下表所示。

| HTTP 状态码 | 涵义                  | 解释说明                                                     |
| ----------- | --------------------- | ------------------------------------------------------------ |
| 200         | OK                    | 用于一般性的成功返回，不可用于请求错误返回                   |
| 201         | Created               | 资源被创建                                                   |
| 202         | Accepted              | 用于资源异步处理的返回，仅表示请求已经收到。对于耗时比较久的处理，一般用异步处理来完成 |
| 204         | No Content            | 此状态可能会出现在 PUT、POST、DELETE 的请求中，一般表示资源存在，但消息体中不会返回任何资源相关的状态或信息 |
| 400         | Bad Request           | 用于客户端一般性错误信息返回, 在其它 4xx 错误以外的错误，也可以使用，错误信息一般置于 body 中 |
| 401         | Unauthorized          | 接口需要授权访问，为通过授权验证                             |
| 403         | Forbidden             | 当前的资源被禁止访问                                         |
| 404         | Not Found             | 找不到对应的信息                                             |
| 500         | Internal Server Error | 服务器内部错误                                               |

​        我们知道 HTTP 共有四个谓词方法，分别为 Get、Post、Put 和 Delete，在之前我们可能更多的是使用 Get 和 Post，对于 Put 和 Delete 方法可能并不会使用。同样的，如果我们需要创建符合 Restful 风格的接口，我们则需要根据这四个 HTTP 方法谓词一些约定俗成的功能定义去定义对应接口的 HTTP 方法。

| HTTP 谓词方法 | 解释说明           |
| ------------- | ------------------ |
| GET           | 获取资源信息       |
| POST          | 提交新的资源信息   |
| PUT           | 更新已有的资源信息 |
| DELETE        | 删除资源           |

​        例如，对于一个获取所有资源的方法，我们可能会定义接口的默认返回 HTTP 状态码为 200 或是 400，当状态码为 200 时，代表数据获取成功，接口可以正常返回数据，当状态码为 400 时，则代表接口访问出现问题，此时则返回错误信息对象。

​        在 ASP.NET Core Web API 中，我们可以通过在 Action 上添加 ProducesResponseType 特性来定义接口的返回状态码。通过 F12 按键我们可以进入 ProducesResponseType 这个特性，可以看到这个特性存在两个构造方法，我们可以只定义接口返回 HTTP 状态码或者是在定义接口返回的状态码时同时返回的具体对象信息。

![定义接口返回状态码](https://user-gold-cdn.xitu.io/2019/7/27/16c327279dfa2db4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        上面给出的接口案例的示例代码如下所示，从下图中可以看到，Swagger 会自动根据我们的 ProducesResponseType 特性来列出我们接口可能返回的 HTTP 状态码和对象信息。这里因为是示例程序，UserListDto 并没有定义具体的属性信息，所以这里显示的是一个不包含任何属性的对象数组。



```
/// <summary>
/// 获取全部的用户信息
/// </summary>
/// <returns></returns>
[HttpGet]
[ProducesResponseType(typeof(IEnumerable<UserListDto>), StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public IActionResult Get()
{
    // 1、获取资源数据

    // 2、判断数据获取是否成功
    if (true)
        return Ok(new List<UserListDto>());
    else
        return BadRequest(new
        {
            statusCode = StatusCodes.Status400BadRequest,
            description = "错误描述",
            msg = "错误信息"
        });
}
复制代码
```



![接口文档](https://user-gold-cdn.xitu.io/2019/7/27/16c3272c8a738bd7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        可能这里你可能会有疑问，当接口返回的 HTTP 状态码为 400 时，返回的信息是什么鬼，与我们定义的错误信息对象字段不同啊？原来，在 ASP.NET Core 2.1 之后的版本中，对于 API 接口返回 400 的 HTPP 状态码会默认返回 ProblemDetails 对象，因为这里我们并没有将接口中的返回 BadRequest 中的错误信息对象作为 ProducesResponseType 特性的构造函数的参数，所以这里就采用了默认的错误信息对象。



​        当然，当接口的 HTTP 返回状态码为 400 时，最终还是会返回我们自定义的错误信息对象，所以这里为了不造成前后端对接上的歧义，我们最好将返回的对象信息也作为参数添加到 ProducesResponseType 特性中。

![自定义错误信息对象](https://user-gold-cdn.xitu.io/2019/7/27/16c327313af57f09?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​        同时，除了上面示例的接口中通过返回 OK 方法和 BadRequest 方法来表明接口的返回 HTTP 状态码，在 ASP.NET Core Web API 中还有下列继承于 ObjectResult 的方法来表明接口返回的状态码，对应信息如下。



| HTTP 状态码 | 方法名称       |
| ----------- | -------------- |
| 200         | OK()           |
| 201         | Created()      |
| 202         | Accepted()     |
| 204         | NoContent()    |
| 400         | BadRequest()   |
| 401         | Unauthorized() |
| 403         | Forbid()       |
| 404         | NotFound()     |

​        **六、使用 Web API 分析器**

​        在上面的示例中，因为我们需要指定接口需要返回的 HTTP 状态码，所以我们需要提前添加好 ProducesResponseType 特性，在某些时候我们可能在代码中添加了一种 HTTP 状态码的返回结果，可是却忘了添加特性描述，那么有没有一种便捷的方式提示我们呢？

​        在 ASP.NET Core 2.2 及以后更新的 ASP.NET Core 版本中，我们可以通过 Nuget 去添加 Microsoft.AspNetCore.Mvc.Api.Analyze 这个包，从而实现对我们的 API 进行分析，首先我们需要将这个包添加到我们的 API 项目中。

```
Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
复制代码
```

​        例如在下面的接口代码中，我们根据用户的唯一标识去寻找用户数据，当获取不到数据的时候，返回的 HTTP 状态码为 400，而我们只添加了 HTTP 状态码为 200 的特性说明。此时，分析器将 HTTP 404 状态代码的缺失特性说明做为一个警告，并提供了修复此问题的选项，我们进行修复后就可以自动添加特性。

```
/// <summary>
/// 获取用户详细信息
/// </summary>
/// <param name="id">用户唯一标识</param>
/// <returns></returns>
[HttpGet("{id}")]
[ProducesResponseType(typeof(UserEditDto), StatusCodes.Status200OK)]
public IActionResult Get(string id)
{
    // 1、根据 Id 获取用户信息
    UserEditDto user = null;

    if (user == null)
        return NotFound();
    else
        return Ok(user);
}
复制代码
```



![Web API 分析](https://user-gold-cdn.xitu.io/2019/7/27/16c327390092a4a1?imageslim)

​        但是，在自动完成文档补全后其实还是需要我们进行一些操作的，例如，如果我们需要指定返回值的 Type 类型，还是需要我们自己手动添加到 ProducesResponseType 特性上的。



​        在进行特性补齐的时候，分析器也帮我们填加了一个 ProducesDefaultResponseType 特性。通过在微软的文档中指向的 Swagger 文档（[Swagger Default Response](https://link.juejin.im/?target=https%3A%2F%2Fswagger.io%2Fdocs%2Fspecification%2Fdescribing-responses%2F%23default)）中可以了解到，如果我们接口不管是什么状态，最终返回的 response 响应结构都是相同的，我们就可以直接使用 ProducesDefaultResponseType 特性来指定 response 的响应结构，而不需要每个 HTTP 状态都添加一个特性。

### 总结

​        在本篇文章中，主要介绍了一些我在使用 ASP.NET Core Web API 的过程中使用到的一些小技巧，以及在以前踩过坑后的一些解决方案，如果对你能有一点的帮助的话，不胜荣幸。同时，如果你有更好的解决方案，或者是针对一些你之前踩过的 Web API 坑的解决方案，也欢迎你在评论区中提出。

> ### 占坑
>
> ​        作者：[墨墨墨墨小宇](https://juejin.im/user/5bd93a936fb9a0224268c11b)
> ​        个人简介：96年生人，出生于安徽某四线城市，毕业于Top 10000000 院校。.NET程序员，枪手死忠，喵星人。于2016年12月开始.NET程序员生涯，微软.NET技术的坚定坚持者，立志成为云养猫的少年中面向谷歌编程最厉害的.NET程序员。
> ​        个人博客：[yuiter.com](https://link.juejin.im/?target=https%3A%2F%2Fyuiter.com)
> ​        博客园博客：[www.cnblogs.com/danvic712](https://link.juejin.im/?target=https%3A%2F%2Fwww.cnblogs.com%2Fdanvic712)

关注下面的标签，发现更多相似文章