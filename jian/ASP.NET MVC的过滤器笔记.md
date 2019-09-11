# ASP.NET MVC的过滤器笔记

2018.12.06 21:41:15字数 378阅读 102

### 过滤器概念

APS.NET MVC中（以下简称“MVC”）的每一个请求，都会分配给相应的控制器和对应的行为方法去处理，而在这些处理的前前后后如果想再加一些额外的逻辑处理。这时候就用到了过滤器。



1、过滤器（Filters）就是向请求处理管道中注入额外的逻辑。提供了一个简单而优雅的方式来实现横切关注点。

2、所谓的过滤器（Filters），MVC框架里面的过滤器完全不同于ASP.NET平台里面的Request.Filters和Response.Filter对象，它们主要是实现请求和响应流的传输。通常我们所说的过滤器是指MVC框架里面的过滤器。

3、过滤器可以注入一些代码逻辑到请求处理管道中，是基于C#的Attribute的实现。当负责调用Action的类ControllerActionInvoker在调用执行Action的时候会检查Action上面的Attribute并查看这些Attribute是否实现了指定的接口，以便进行额外的代码注入处理

### 过滤器分类

MVC支持的过滤器类型有四种，分别是：Authorization(授权),Action（行为）,Result（结果）和Exception（异常）。如下表，

| 过滤器类型    | 接口                 | 描述                                                         |
| ------------- | -------------------- | ------------------------------------------------------------ |
| Authorization | IAuthorizationFilter | 此类型（或过滤器）用于限制进入控制器或控制器的某个行为方法   |
| Exception     | IExceptionFilter     | 用于指定一个行为，这个被指定的行为处理某个行为方法或某个控制器里面抛出的异常 |
| Action        | IActionFilter        | 用于进入行为之前或之后的处理                                 |
| Result        | IResultFilter        | 用于返回结果的之前或之后的处理                               |

但是默认实现它们的过滤器只有三种，分别是Authorize（授权），ActionFilter，HandleError（错误处理）；各种信息如下表所示

| 过滤器       | 类名                  | 实现接口                     | 描述                                                         |
| ------------ | --------------------- | ---------------------------- | ------------------------------------------------------------ |
| ActionFilter | AuthorizeAttribute    | IAuthorizationFilter         | 此类型（或过滤器）用于限制进入控制器或控制器的某个行为方法   |
| HandleError  | HandleErrorAttribute  | IExceptionFilter             | 用于指定一个行为，这个被指定的行为处理某个行为方法或某个控制器里面抛出的异常 |
| 自定义       | ActionFilterAttribute | IActionFilter和IResultFilter | 用于进入行为之前或之后的处理或返回结果的之前或之后的处理     |