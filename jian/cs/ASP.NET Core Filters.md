# ASP.NET Core Filters

阅读 201

收藏 7

2018-08-12

原文链接：[beckjin.com](https://link.juejin.im/?target=http%3A%2F%2Fbeckjin.com%2F2018%2F08%2F12%2Faspnet-filter%2F)

[在实时音视频中应用深度学习，你需要了解这些juejin.im](https://juejin.im/post/5d9da1446fb9a04df26c2145)

ASP.NET MVC 中的过滤器（Filter）是 AOP（面向切面编程） 思想的一种实现，供我们在执行管道的特定阶段执行代码，通过使用过滤器可以实现 短路请求、缓存请求结果、日志统一记录、参数合法性验证、异常统一处理、返回值格式化 等等，同时使业务代码更加简洁单纯，避免很多重复代码。

### 过滤器执行流程

MVC 在选择要执行的 Action 方法后，会执行过滤器管道，流程如下图：

![MVC 调用管道](https://user-gold-cdn.xitu.io/2018/8/12/1652a0111c00fe6f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 过滤器作用域

过滤器作用域设置是非常灵活的，可以选择为：

1. 全局有效（*整个 MVC 应用程序下的每一个 Action*）；
2. 仅对某些 Controller 有效 (*控制器内所有的 Action* )；
3. 仅对某些 Action 有效;

### 过滤器类型

过滤器分为 Authorization Filter、Resource Filter、Action Filter、Exception Filter、Result Filter，每种过滤器都有其应用场景，不同类型的过滤器会在执行管道的不同阶段运行，我们需要根据实际情况来选择使用。

#### Authorization Filter

授权过滤器在过滤器管道中第一个被执行，通常用于验证请求的合法性（*通过实现接口 IAuthorizationFilter or IAsyncAuthorizationFilter*）

#### Resource Filter

资源过滤器在过滤器管道中第二个被执行，通常用于请求结果的缓存和短路过滤器管道（*通过实现接口 IResourceFilter or IAsyncResourceFilter*）

#### Action Filter

Acioin 过滤器可设置在调用 Acioin 方法之前和之后执行代码，如：请求参数的验证（*通过实现接口 IActionFilter or IAsyncActionFilter*）

#### Exception Filter

程序异常信息处理（*通过实现接口 IExceptionFilter or IAsyncExceptionFilter*）

#### Result Filter

Action 执行完成后执行，对执行结果格式化处理（*通过实现接口 IResultFilter or IAsyncResultFilter*）

```
public class AddHeaderResultFilter : IResultFilter
{
	public void OnResultExecuted(ResultExecutedContext context)
	{
		Console.WriteLine("AddHeaderResultFilter:OnResultExecuted");
	}

	public void OnResultExecuting(ResultExecutingContext context)
	{
		context.HttpContext.Response.Headers.Add("ResultFilter", new string[] { "AddHeader" });

		context.Result = new ObjectResult(new ApiResult<string>()
		{
			Code = Enum.ResultCode.Success,
			Data = "我是 AddHeaderResultFilter 修改后的值"
		});
	}
}
```

#### Filter Attributes

将过滤器接口的实现以特性（*Attributes*） 的方式使用是非常方便的，过滤器特性可应用于 Controller 和 Action 。框架包含了内置的基于特性的过滤器，我们可以直接继承或另外定制。

继承内置的 ExceptionFilterAttribute：

```
public class CustomExceptionFilterAttribute : ExceptionFilterAttribute
{
	public override void OnException(ExceptionContext context)
	{
		context.Result = new ObjectResult(new ApiResult()
		{
			Code = Enum.ResultCode.Exception,
			ErrorMessage = $"CustomExceptionFilterAttribute: {context.Exception.Message}"
		});
	}
}
```



```
[CustomExceptionFilter]
public ApiResult ExceptionAttributeTest()
{
	throw new Exception("Boom");
}
```

定制 ResourceFilterAttribute：

```
public class ShortCircuitingResourceFilterAttribute : Attribute, IResourceFilter
{
	public void OnResourceExecuted(ResourceExecutedContext context)
	{
	}

	public void OnResourceExecuting(ResourceExecutingContext context)
	{
		context.Result = new ObjectResult(new ApiResult<string>()
		{
			Code = Enum.ResultCode.Failed,
			Data = "我是 ShortCircuitingResourceFilterAttribute 的返回值"
		});
	}
}
```



### 过滤器实战

问题：在开发中都存在这样的场景，方法参数需要添加一些 if 的判断，不合法直接返回错误信息，方法体加个 try\catch，捕获到异常后记录日志，而这些基本是每一个接口方法必须的，实际情况下我们可能是 ctrl+c & ctrl+v ，部分代码微调，搞定，并不会花什么时间，但我们可以看看整个文件的代码，大部分都是一样的架子，头部验证，尾部捕获，中间调用主逻辑层的方法，满满的重复代码，而且如果后期要调整基础架子，每个方法都得来一遍，那必须得蛋疼死。

解决方案： Action Filter + Exception Filter

1. 创建一个 Filter，同时实现 IAsyncActionFilter 和 IAsyncExceptionFilter：

   ```
   public class XXXActionFilter : IAsyncActionFilter, IAsyncExceptionFilter
   {
   	private IDictionary<string, object> _actionArguments;
   
   	public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
   	{
   		// 如果参数验证不通过，返回参数错误的信息
   		if (!context.ModelState.IsValid)
   		{
   			var errorMessages = new List<string>();
   			foreach (var key in context.ModelState.Keys)
   			{
   				var state = context.ModelState[key];
   				var errorModel = state?.Errors?.First();
   				if (errorModel != null)
   					errorMessages.Add($"{key}:{errorModel.ErrorMessage}");
   			}
   			var result = new ApiResult()
   			{
   				Code = ResultCode.ArgumentError,
   				Message = errorMessages.Join(",")
   			};
   			context.Result = new ObjectResult(result);
   			return;
   		}
   
   		// 保存下来，ExceptionContext 中取不到，如果出异常了需要记录请求参数
   		_actionArguments = context.ActionArguments;
   
   		await next();
   	}
   
   	public async Task OnExceptionAsync(ExceptionContext context)
   	{
   		
   		// 记录异常日志 
   		// some code..........
   		
   		var result = new ApiResult()
   		{
   			Code = ResultCode.Exception,
   			Message = $"{context.ActionDescriptor.RouteValues["action"]} Exception")
   		};
   		context.Result = new ObjectResult(result);
   		await Task.CompletedTask;
   	}
   }
   ```

2. Startup.cs 的 ConfigureServices 中添加 XXXActionFilter 为全局有效：

   ```
   services.AddMvc(options =>
   {
   	options.Filters.Add<XXXActionFilter>();
   });
   ```

3. Controller 中的添加测试方法 ：

   ```
   public ApiResult ActionTest(XXXRequest request)
   {
   	if (request.Id == "1")
   	{
   		throw new Exception("xxxx");
   	}
   	return new ApiResult<string>()
   	{
   		Code = Enum.ResultCode.Success,
   		Data = "ActionTest"
   	};
   }
   ```

   ```
   public class XXXRequest
   {
   	[Required]
   	public string Id { get; set; }
   }
   ```

调用 ActionTest 进行测试，如果 id 没传，会进去 OnActionExecutionAsync 的 context.ModelState.IsValid 为 fasle 的情况：

![invalid](https://user-gold-cdn.xitu.io/2018/8/12/1652a0124f26bd6d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如果 id 为1，会进入 OnExceptionAsync：

![exception](https://user-gold-cdn.xitu.io/2018/8/12/1652a011fcd3224c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

其他情况下正常返回：

![success](https://user-gold-cdn.xitu.io/2018/8/12/1652a011fd756eb2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 参考链接

- [Filters in ASP.NET Core](https://link.juejin.im/?target=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Faspnet%2Fcore%2Fmvc%2Fcontrollers%2Ffilters%3Fview%3Daspnetcore-2.1)
- [ASP.NET Core 中文文档 第四章 MVC（4.3）过滤器](https://link.juejin.im/?target=https%3A%2F%2Fwww.cnblogs.com%2FdotNETCoreSG%2Fp%2Faspnetcore-4_4_3-filters.html)