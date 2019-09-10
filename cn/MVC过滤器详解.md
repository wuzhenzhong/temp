# MVC过滤器详解



APS.NET MVC中（以下简称“MVC”）的每一个请求，都会分配给相应的控制器和对应的行为方法去处理，而在这些处理的前前后后如果想再加一些额外的逻辑处理。这时候就用到了过滤器。

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

　　下面介绍的过滤器中，除了上面这几种外，还多加一种过滤器OutputCache

 

### 1 授权过滤器Authorize

 

#### 1.1 默认Authorize使用

　　现在在网上无论是要求身份验证的地方多得是，发邮件，买东西，有时候就算吐个槽都要提示登录的。这里的某些操作，就是要经过验证授权才被允许。在MVC中可以利用Authorize来实现。例如一个简单的修改密码操作

```c#
`[Authorize]``public` `ActionResult ChangePassword()``{``      ``return` `View();``}`
```

　　

它需要用户通过了授权才能进入到这个行为方法里面，否则硬去请求那个页面的话，只会得到这个结果

![img](https://images0.cnblogs.com/blog/441298/201309/27080730-561df16854334b0fb03ae7aacb028d9a.jpg)

如果要通过验证，通过调用FormsAuthentication.SetAuthCookie方法来获得授权，登陆的页面如下

```html
`@model FilterTest.Models.LogInModel``@{``    ``Layout = ``null``;``}` `<!DOCTYPE html>` `<html>``<head>``    ``<title>Login</title>``</head>``<body>``    ``<div>``    ``@``using``( Html.BeginForm()){``       ``<div>``        ``ID:@Html.TextBoxFor(m=>m.UserName)``        ``<br />``        ``Password:@Html.PasswordFor(m => m.Password)``        ``<br />``        ``<input type=``"submit"` `value=``"login"` `/>``       ``</div>``    ``}``    ``</div>``</body>``</html>`
```

　　行为方法如下

```c#
`[HttpPost]``//这里用了谓词过滤器，只处理POST的请求``        ``public` `ActionResult Login(LogInModel login)``        ``{``            ``if` `(login.UserName == ``"admin"` `&& login.Password == ``"123456"``)``            ``{``                ``FormsAuthentication.SetAuthCookie(login.UserName, ``false``);``                ``return` `Redirect(``"/Customer/ChangePassword"``);``            ``}` `            ``return` `View();``        ``}`
```

　　当然有登录也要有注销，因为注销是在登陆之后发生的，没登陆成功也就没有注销，所以注销的行为方法也要加上Authorize过滤器，注销调用的是FormsAuthentication.SignOut方法，代码如下

```c#
`[Authorize]``public` `ActionResult LogOut()``{``     ``FormsAuthentication.SignOut();``     ``return` `Redirect(``"/Customer/Login"``);``}`
```

　　

#### 1.2 自定义授权

我们不一定要用MVC默认的Authorize授权验证规则，规则可以自己来定，自定义授权过滤器可以继承AuthorizeAttribute这个类，这个类里面有两个方法是要重写的

- ​         bool AuthorizeCore(HttpContextBase httpContext)：这里主要是授权验证的逻辑处理，返回true的则是通过授权，返回了false则不是。
- ​         void HandleUnauthorizedRequest(AuthorizationContext filterContext)：这个方法是处理授权失败的事情。

这里就定义了一个比较骑呢的授权处理器，当请求的时候刚好是偶数分钟的，就通过可以获得授权，反之则不通过。当授权失败的时候，就会跳转到登陆页面了。

```c#
`public` `class` `MyAuthorizeAttribute:AuthorizeAttribute``    ``{``        ` `        ``protected` `override` `bool` `AuthorizeCore(HttpContextBase httpContext)``        ``{``            ``//return base.AuthorizeCore(httpContext);``            ``return` `DateTime.Now.Minute % 2 == 0``        ``}` `        ` `        ``protected` `override` `void` `HandleUnauthorizedRequest(AuthorizationContext filterContext)``        ``{``            ``filterContext.HttpContext.Response.Redirect(``"/Customer/Login"``);``            ` `            ``//base.HandleUnauthorizedRequest(filterContext);``        ``}``    ``}`
```

　　然后用到一个行为方法上，

```c#
`[MyAuthorize]``public` `ActionResult ShowDetail()``{``     ``return` `View();``}`
```

每当偶数分钟的时候就可以访问得到这个ShowDetail的视图，否则就会跳到了登陆页面了。



### 2 处理错误过滤器HandleError

#### 2.1 默认HandleError使用

在往常的开发中，想到异常处理的马上就会想到try/catch/finally语句块。在MVC里面，万一在行为方法里面抛出了什么异常的，而那个行为方法或者控制器有用上HandleError过滤器的，异常的信息都会在某一个视图显示出来，这个显示异常信息的视图默认是在Views/Shared/Error

这个HandleError的属性如下

| 属性名称      | 类型   | 描述                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| ExceptionType | Type   | 要处理的异常的类型，相当于Try/Catch语句块里Catch捕捉的类型，如果这里不填的话则表明处理所有异常 |
| View          | String | 指定需要展示异常信息的视图，只需要视图名称就可以了，这个视图文件要放在Views/Shared文件夹里面 |
| Master        | String | 指定要使用的母版视图的名称                                   |
| Order         | Int    | 指定过滤器被应用的顺序，默认是-1，而且优先级最高的是-1       |

这个Order属性其实不只这个HandleError过滤器有，其优先级规则跟其他过滤器的都是一样。

下面则故意弄一个会抛异常的行为方法

```c#
`[HandleError(ExceptionType = ``typeof``(Exception))]``        ``public` `ActionResult ThrowErrorLogin()``        ``{``            ``throw` `new` `Exception(``"this is ThrowErrorLogin Action Throw"``);``        ``}`
```

　　光是这样还不够，还要到web.config文件中的<system.web>一节中添加以下代码

```xml
`<customErrors mode=``"On"` `/>`
```

　　

因为默认的开发模式中它是关闭的，要等到部署到服务器上面才会开启，让异常信息比较友好的用一个视图展现。

　　像这里访问ThrowErrorLogin视图时，由于抛出了一次，就转到了一个特定的视图

![img](https://images0.cnblogs.com/blog/441298/201309/27081229-cbd0f1e0c22949afa8a6ee0fdbcb764b.jpg)

　　在这里看到的异常视图是这样的，除了用这个建项目时默认生成的异常视图之外，我们还可以自己定义异常视图，视图里面要用到的异常信息，可以通过@Model获取，它是一个ExceptionInfo类型的实例，例如这里建了一个异常视图如下

```
`@{``    ``Layout = ``null``;``}` `<!DOCTYPE html>` `<html>``<head>``    ``<title>MyErrorPage</title>``</head>``<body>``    ``<div>``    ``<p>``        ``There was a <b>@Model.Exception.GetType().Name</b>``        ``while` `rendering <b>@Model.ControllerName</b>'s``        ``<b>@Model.ActionName</b> action.``    ``</p>``    ``<p style=``"color:Red"``>``        ``<b>@Model.Exception.Message</b>``    ``</p>``    ``<p>Stack trace:</p>``    ``<pre style=``" background-color:Orange"``>@Model.Exception.StackTrace</pre>``        ``</div>``</body>``</html>`
```

　　它存放的路径是~/Views/Shared里面，像上面的行为方法如果要用异常信息渲染到这个视图上面，在控制器的处改成这样就可以了

```
`[HandleError(ExceptionType = ``typeof``(Exception), View = ``"MyErrorPage"``)]`
```

　　

![img](https://images0.cnblogs.com/blog/441298/201309/27081349-74f2d85125214d0799f814b6d73b2929.jpg)

#### 2.2 自定义错误异常处理

　　这里的错误处理过滤器也可以自己来定义，做法是继承HandleErrorAttribute类，重写void OnException(ExceptionContext filterContext)方法，这个方法调用是为了处理未处理的异常，例如

```c#
`public` `override` `void` `OnException(ExceptionContext filterContext)``        ``{``            ``//base.OnException(filterContext);``            ``if` `(!filterContext.ExceptionHandled &&``                ``filterContext.Exception.Message == ``"this is ThrowErrorLogin Action Throw"``)``            ``{``                ``filterContext.ExceptionHandled=``true``;``                ``filterContext.HttpContext.Response.Write(``"5洗ten No Problem<br/>"` `+``                    ``filterContext.Exception.ToString());``            ``}``        ``}`
```

　　

这里用到的传入了一个ExceptionContext的对象，既可以从它那里获得请求的信息，又可以获取异常的信息，它部分属性如下

| 属性名称         | 类型             | 描述                                                         |
| ---------------- | ---------------- | ------------------------------------------------------------ |
| ActionDescriptor | ActionDescriptor | 提供详细的操作方法                                           |
| Result           | ActionResult     | 结果的操作方法，过滤器可以取消，要求将此属性设置为一个非空值 |
| Exception        | Exception        | 未处理的异常                                                 |
| ExceptionHandled | bool             | 另一个过滤器，则返回true，如果有明显的异常处理               |


 　　这里的ExceptionHandler属性要提一下的是，如果这个异常处理完的话，就把它设为true，那么即使有其他的错误处理器捕获到这个异常，也可以通过ExceptionHandler属性判断这个异常是否经过了处理，以免重复处理一个异常错误而引发新的问题。

### 3 OutputCache过滤器

　　OutputCache过滤器用作缓存，节省用户访问应用程序的时间和资源，以提高用户体验，可这个我试验试不出它的效果。留作笔记记录一下。OutputCacheAttribute这个类有以下属性

 

| 属性名称    | 类型                | 描述                                                         |
| ----------- | ------------------- | ------------------------------------------------------------ |
| Duration    | int                 | 缓存的时间，以秒为单位，理论上缓存时间可以很长，但实际上当系统资源紧张时，缓存空间还是会被系统收回。 |
| VaryByParam | string              | 以哪个字段为标识来缓存数据，比如当“ID”字段变化时，需要改变缓存（仍可保留原来的缓存），那么应该设VaryByParam为"ID"。这里你可以设置以下几个值： * = 任何参数变化时，都改变缓存。 none = 不改变缓存。 以分号“;”为间隔的字段名列表 = 列表中的字段发生变化，则改变缓存。 |
| Location    | OutputCacheLocation | 缓存数据放在何处。默认是Any,其他值分别是Client，Downstream，Server，None，ServerAndClient |
| NoStore     | bool                | 用于决定是否阻止敏感信息的二级存储。                         |

　　例如一个OutputCache过滤器可以这样使用

```c#
`[OutputCache(Location= System.Web.UI.OutputCacheLocation.Client,Duration=60)]``        ``public` `ActionResult Login()``        ``{``            ``return` `View();``        ``}`
```

　　或者有另外一种使用方式——使用配置文件，在<system.web>节点下添加以下设置

```xml
`<caching>``      ``<outputCacheSettings>``        ``<outputCacheProfiles>``          ``<add name=``"testCache"` `location=``"Client"` `duration=``"60"``/>``        ``</outputCacheProfiles>``      ``</outputCacheSettings>``    ``</caching>`
```

　　使用控制的时候就这样

```c#
`[OutputCache(CacheProfile=``"testCache"``)]``        ``public` `ActionResult Login()``        ``{``            ``return` `View();``        ``}`
```

　　

### 4 自定义过滤器

　　万一前面介绍的过滤器也满足不了需求，要在行为方法执行返回的前前后后定义自己的处理逻辑的话，这个自定义过滤器就应该能派上用场了。若要自定义一个过滤器，则要继承ActionFilterAttribute类，这个类是一个抽象类，实现了IActionFilter和IResultFilter接口，主要通过重写四个虚方法来达到在行为方法执行和返回的前后注入逻辑

| 方法              | 参数                   | 描述                 |
| ----------------- | ---------------------- | -------------------- |
| OnActionExecuting | ActionExecutingContext | 在行为方法执行前执行 |
| OnActionExecuted  | ActionExecutedContext  | 在行为方法执行后执行 |
| OnResultExecuting | ResultExecutingContext | 在行为方法返回前执行 |
| OnResultExecuted  | ResultExecutedContext  | 在行为方法返回后执行 |

　　四个方法执行顺序是OnActionExecuting——>OnActionExecuted——>OnResultExecuting——>OnResultExecuted。上面四个方法的参数都是继承基ContollorContext类。例如下面定义了一个自定义的过滤器

```c#
`public` `class` `MyCustomerFilterAttribute : ActionFilterAttribute``    ``{``        ``public` `string` `Message { ``get``; ``set``; }` `        ``public` `override` `void` `OnActionExecuted(ActionExecutedContext filterContext)``        ``{``            ``base``.OnActionExecuted(filterContext);``            ``filterContext.HttpContext.Response.Write(``string``.Format( ``"<br/> {0} Action finish Execute....."``,Message));``        ``}` `        ``public` `override` `void` `OnActionExecuting(ActionExecutingContext filterContext)``        ``{``            ``CheckMessage(filterContext);``            ``filterContext.HttpContext.Response.Write(``string``.Format(``"<br/> {0} Action start Execute....."``, Message));``            ``base``.OnActionExecuting(filterContext);``        ``}` `        ``public` `override` `void` `OnResultExecuted(ResultExecutedContext filterContext)``        ``{``            ``filterContext.HttpContext.Response.Write(``string``.Format(``"<br/> {0} Action finish Result....."``, Message));``            ``base``.OnResultExecuted(filterContext);``        ``}` `        ``public` `override` `void` `OnResultExecuting(ResultExecutingContext filterContext)``        ``{``            ``filterContext.HttpContext.Response.Write(``string``.Format(``"<br/> {0} Action start Execute....."``, Message));``            ``base``.OnResultExecuting(filterContext);``        ``}` `        ``private` `void` `CheckMessage(ActionExecutingContext filterContext)``        ``{``            ``if``(``string``.IsNullOrEmpty( Message)||``string``.IsNullOrWhiteSpace(Message))``                ``Message = filterContext.Controller.GetType().Name + ``"'s "` `+ filterContext.ActionDescriptor.ActionName;``        ``}``    ``}`
```

　　使用它的行为方法定义如下

```c#
`[MyCustomerFilter]``       ``public` `ActionResult CustomerFilterTest()``       ``{``           ``Response.Write(``"<br/>Invking CustomerFilterTest Action"``);``           ``return` `View();``       ``}`
```

　　

执行结果如下

![img](https://images0.cnblogs.com/blog/441298/201309/27081916-016b1461347f420bbbb018dcad895fd2.jpg)

这个就证明了上面说的顺序。

当控制器也使用上这过滤器时，而行为方法不使用时，结果如下

![img](https://images0.cnblogs.com/blog/441298/201309/27082002-2556dd403df64cffb0f6f79522092322.jpg)

如果控制器和行为方法都使用了过滤器，理论上是显示上面两个结果的有机结合。但实际不然，因为在定义过滤器的时候还少了一个特性：[AttributeUsage(AttributeTargets.All, AllowMultiple = true)]，把这个加在MyCustomerFilterAttribute就行了。

```c#
`[AttributeUsage(AttributeTargets.All, AllowMultiple = ``true``)]``//多次调用``    ``public` `class` `MyCustomerFilterAttribute : ActionFilterAttribute``    ``{``        ``……``    ``}`
```

　　

![img](https://images0.cnblogs.com/blog/441298/201309/27082119-6f26257473b94bb88f3df1eaf8ebe3e7.jpg)

　　由这幅图可以看出，同一个过滤器分别用在了控制器和行为方法中，执行同一个方法时都会有先后顺序，如果按默认值（不设Order的情况下），一般的顺序是由最外层到最里层，就是“全局”——>“控制器”——>“行为方法”；而特别的就是错误处理的过滤器，由于异常是由里往外抛的，所以它的顺序刚好也反过来：“行为方法”——>“控制器”——>“全局”。

　　既然这里有提到全局的过滤器，那么全局的过滤器是在Global.asax文件里面的RegisterGlobalFilters(GlobalFilterCollection filters)中设置的

```c#
`public` `static` `void` `RegisterGlobalFilters(GlobalFilterCollection filters)``        ``{``            ``filters.Add(``new` `HandleErrorAttribute());``            ``filters.Add(``new` `MyFilters.MyCustomerFilterAttribute() { Message=``"Global"``});``//全局过滤器``        ``}`
```

　　

这里它默认也添加了一个错误处理的过滤器来处理整个MVC应用程序所抛出的异常。

转自：http://www.cnblogs.com/HopeGi/p/3342083.html#commentform

 

声明：本文纯属个人随手笔记，如果对您有参考价值我十分开心，如果有存在错误，或者有更好的解决办法也麻烦您留言告诉我，大家共同成长，切勿恶言相。 欢迎加入MSDN技术交流群：235937854，一起发现知识、了解知识、学习知识、分享知识



分类: [MVC](https://www.cnblogs.com/imhaiyang/category/729085.html)