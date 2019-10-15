# [译] 在 ASP.NET Core 中使用 SignalR

> 原文：[weblogs.asp.net/ricardopere…](https://link.juejin.im/?target=https%3A%2F%2Fweblogs.asp.net%2Fricardoperes%2Fsignalr-in-asp-net-core)

> 作者：Ricardo Peres

> 转载自：oopsguy.com

## 介绍

[SignalR](https://link.juejin.im/?target=http%3A%2F%2Fwww.asp.net%2Fsignalr) 是一个用于实现实时功能的 Microsoft .NET 库。它使用了多种技术来实现服务器与客户端间的双向通信，服务器可以随时将消息推送到连接的客户端。

现在你可以在 ASP.NET Core 预发行版本中体验它（译者：根据原文的发布时间）。我已经介绍过几次 SignalR 了。

## 安装

你将需要安装 [Microsoft.AspNetCore.SignalR.Client](https://link.juejin.im/?target=https%3A%2F%2Fwww.nuget.org%2Fpackages%2FMicrosoft.AspNetCore.SignalR.Client) 和 [Microsoft.AspNetCore.SignalR](https://link.juejin.im/?target=https%3A%2F%2Fwww.nuget.org%2Fpackages%2FMicrosoft.AspNetCore.SignalR) 这两个 Nuget 预发行包。此外，你还需要 [NPM](https://link.juejin.im/?target=https%3A%2F%2Fwww.npmjs.com%2F)（Node 包管理器）。安装 NPM 后，你需要获取 [@aspnet/signalr-client](https://link.juejin.im/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2F%40aspnet%2Fsignalr-client) 包，之后再从 **node_modules@aspnet\signalr-client\dist\browser** 文件夹中获取 **signalr-client-1.0.0-alpha1-final.js** 文件（版本可能不同），并将其放置在 **wwwroot** 文件夹下，以便可以从页面引用到它。

在使用前，我们需要在 **ConfigureServices** 中注册必须的服务：

```
services.AddSignalR();
复制代码
```

我们将实现一个简单的聊天客户端，因此要在 **Configure** 方法中注册一个 ChatHub：

```
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("chat");
});
复制代码
```

注意：**UseSignalR** 必须在 **UseMvc** 之前调用！

如果你有不同的端点，可以为每个 hub 执行此操作。

在视图或布局文件中，添加对 **signalr-client-1.0.0-alpha1-final.js** 文件的引用：

```
<script src="libs/signalr-client/signalr-client-1.0.0-alpha1-final.js"></script>
复制代码
```

## 实现 Hub

该 hub 是一个继承了 **Hub** 的类。你可在其中添加 JavaScript 可能用到的方法。我们将实现一个 chat hub：

```
public class ChatHub : Hub
{
    public async Task Send(string message)
    {
        await this.Clients.All.InvokeAsync("Send", message);
    }
}
复制代码
```

如上所述，我们有一个方法（**Send**），在本例中，它采用了单参数（**message**）。你不需要在广播调用（**InvokeAsync**）上传递相同的参数，可以发送任何你想要传递的参数。

回到客户端部分，在引用 SignalR JavaScript 文件后添加如下代码：

```
   <script>
     
        var transportType = signalR.TransportType.WebSockets;
        //can also be ServerSentEvents or LongPolling
        var logger = new signalR.ConsoleLogger(signalR.LogLevel.Information);
        var chatHub = new signalR.HttpConnection(`http://${document.location.host}/chat`, { transport: transportType, logger: logger });
        var chatConnection = new signalR.HubConnection(chatHub, logger);
     
        chatConnection.onClosed = e => {
            console.log('connection closed');
       };
    
       chatConnection.on('Send', (message) => {
           console.log('received message');
       });
    
       chatConnection.start().catch(err => {
           console.log('connection error');
       });
    
       function send(message) {
           chatConnection.invoke('Send', message);
       }
    
</script>
复制代码
```

请注意：

1. 创建指向当前 URL 的连接后，链接添加了 **chat** 后缀，这与在 **MapHub** 中注册的一致
2. 它使用特定的传输方式进行初始化（本例中是 **WebSockets**），但这不是必需的，也就是说，你可以让 SignalR 自己采用合适的方式。对于某些操作系统（如 Windows 7），比如你可能无法使用 [WebSockets](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FWebSockets_API)，因此你必须选择 [LongPolling](https://link.juejin.im/?target=https%3A%2F%2Fwww.pubnub.com%2Fblog%2F2014-12-01-http-long-polling%2F) 或 [ServerSentEvents](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FServer-sent_events)
3. 需要通过调用 **start** 来初始化连接
4. handler 的 **Send** 方法与 **ChatHub** 的 **Send** 方法有相同的单个参数（message）

所以，每当有人访问此页面并调用 JavaScript **send**函数时，它将调用 **ChatHub** 类上的 **Send** 方法。该类基本上会向所有连接的客户端（**Clients.All**）广播此消息。也可以将消息发送到特定的组：

```
await this.Clients.Group("groupName").InvokeAsync("Send", message);
复制代码
```

或特定客户端：

```
await this.Clients.Client("id").InvokeAsync("Send", message);
复制代码
```

你如果想启用身份验证，可以添加一个由连接 ID 和 **ClaimPrincipal** 标识的用户，如下所示：

```
public override Task OnConnectedAsync()
{
    this.Groups.AddAsync(this.Context.ConnectionId, "groupName");

    return base.OnConnectedAsync();
}
复制代码
```

**OnConnectedAsync** 在新用户连接时将被调用。当有人断开连接时，**OnDisconnectedAsync** 将被调用：

```
public override Task OnDisconnectedAsync(Exception exception)
{
    return base.OnDisconnectedAsync(exception);
}
复制代码
```

如果在断开连接时发生了异常，则 **exception** 参数将为非空值。

只有当前用户进行身份验证时， **Context** 属性才会提供 **ConnectionId** 和 **User** 两个属性。**ConnectionId** 始终被设置为同一个用户，不会改变。 另外，假设你想通过定时器 hub 将定时器 tick 发送到所有连接的客户端，则可以在 **Configure** 方法中执行此操作：

```
TimerCallback callback = (x) => {
    var hub = serviceProvider.GetService<IHubContext<TimerHub>>();
    hub.Clients.All.InvokeAsync("Notify", DateTime.Now);
};

var timer = new Timer(callback);
timer.Change(TimeSpan.FromSeconds(0), TimeSpan.FromSeconds(10));
复制代码
```

我们启动了一个 **Timer**，从那里我们得到了一个定时器 hub 的引用，并使用当前时间戳调用其 **Notify** 方法。**TimerHub** 类只是这样：

```
public class TimerHub : Hub
{
}
复制代码
```

请注意，此类没有公共方法，因为它不是由 JavaScript 调用，它仅用于从外部广播消息（**Timer** 回调）。

## 将消息发送到 Hub

最后，我们还可以将消息从外部发送到 hub。当使用控制器时，你需要注入一个 **IHubContext** 实例，你可以在里面发送消息到 hub，然后将其广播：

```
private readonly IHubContext<ChatHub> _context;

[HttpGet("Send/{message}")]
public IActionResult Send(string message)
{
    //for everyone
    this._context.Clients.All.InvokeAsync("Send", message);
    //for a single group
    this._context.Clients.Group("groupName").InvokeAsync("Send", message);
    //for a single client
    this._context.Clients.Client("id").InvokeAsync("Send", message);

    return this.Ok();
}
复制代码
```

需要注意的是，这与访问 **ChatHub** 类不同，实现起来并不简单，你需要使用 chat hub 的连接才行。

## 结论

SignalR 尚未发布，仍可能会有一些变化。在以后的文章中，我将更详细地介绍 SignalR，包括其可扩展性机制和一些更高级的使用场景。敬请期待！