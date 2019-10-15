# 在 Docker 上运行. NET Core

阅读 1467

收藏 35

2016-06-09

原文链接：[dockone.io](https://link.juejin.im/?target=http%3A%2F%2Fdockone.io%2Farticle%2F1369)

[在实时音视频中应用深度学习，你需要了解这些juejin.im](https://juejin.im/post/5d9da1446fb9a04df26c2145)


【编者的话】本文为Jurgis Pasukonis在medium.com博客中发布的关于在Docker上运行.NET Core的文章，介绍了目前.NET Core在Docker上的开源情况及部分演示。Jurgis目前是TRAFI公司的CTO。



为了得到新玩具的这些体验，我已经提出了如下计划：

1. 在MAC上通过.NET Core和C#创建一个“hello world”的网络服务。
2. 在Docker上运行
3. 部署到AWS容器服务上





### .NET Core

我之所以决定在MAC OSX而不是在Windows上运行是为了让它更有意思（再加上运行Docker的时候会更简单）安装.NET是非常便捷的，我们只要照着以下步骤做。

> 
> [.NET Core安装步骤](https://link.juejin.im/?target=https%3A%2F%2Fwww.microsoft.com%2Fnet%2Fcore%23macosx)

这给了你一个dotnet的命令行工具可以让你不用任何IDE来创建，构建，运行项目。接下来是告诉你如何创建并运行一个新的“Hello World”控制台应用程序：

如果你喜欢纯文本编辑器，那么这就是你需要的。我喜欢漂亮的IDE体验，幸运地是编辑器可以运行在Windows，OSX和Linux上（不用将其与Visual Studio IDE混淆，这是只有Windows才能用的）。Visual Studio Code是一个轻量但功能强大的通用文本编辑器，拥有多种语言的插件支持，这其中也包括了C#。重申，它可以很轻松地从Visual Studio Code网站进行下载和安装：
然后你应该可以在启动之后添加C#的扩展（按F1然后输入“install extension”）。你最好读一下这样可以有更好的主意知道VS Code是如何工作的：
一旦你在VS Code里打开.NET项目，你可以看到突出显示，自动完成的语句和调试！

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F71fb7319d2cdfdaf611eaf52b1f379cd.png)


这就是你去开发一个完全成熟的.NET项目需要的所有工具-在Windows，OSX和Linux上！

## 第二步：ASP.NET Core

我们的目标是要一个Web服务而不是一个控制台应用程序。对于.NET Core来说，额外有一个精巧的Web框架的ASP.NET Core（相比原来的ASP.NET已经完全重写了）。我们可以照着以下步骤将我们的测试应用程序转换成网页服务：

> ASP.NET Core入门指南
>
> 1. 对project.json添加 Microsoft.AspNetCore.Server.Kestrel依赖。
> 2. 对Startup.cs类添加样本请求处理。
> 3. 在Main()中启动网页服务器。

[![3.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Fcf3e817cf603461305f2c79743d27599.png)


是的，在命令行中执行dotnet run或者在VS Code中用F5可以启动虚拟主机监听HTTP的请求。

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F575055b0ea4d60577af8a3c5ece3ae60.png)



[![5.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Fa828c52113e5dc72c0a3255f14d0e095.png)


考虑一下——不需要IIS，不需要Windows，不需要安装，你在10行代码中就获得了一个完整的独立的Web应用程序。

## 小插曲：NuGet/.NET Framework/.NET Core/.NET Standard，etc

到目前为止，我还没有真正解释什么是.NET Core以及它与通常的.NET框架的联系。当然这两者有很多的变化，而且经常会把他们弄混淆（不争的事实是一些工具和命名已经从Alpha到beta再到RC1甚至到RC2的版本中被改变了）。这里是[官方的概述](https://link.juejin.im/?target=https%3A%2F%2Fdotnet.github.io%2Fabout%2Foverview.html)

这是上手的一些基本信息，当然，还有更多的东西需要去学。

## 第三步：配置Docker

让我们试着在我们的Docker.Full上运行我们的.NET网页服务-在这之前，我从来没有在Mac上运行Docker。让我们通过安装Docker来开始：

> 
> [在Mac OS X上安装Docker](https://link.juejin.im/?target=https%3A%2F%2Fdocs.docker.com%2Fmac%2Fstep_one%2F)

如果进展顺利，可以运行**docker run hello-world**

[![7.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Ff14337b159b78ed38242554978f7dc23.png)


接下来让我们返回.NET，这是一个官方的，让我们运行它：

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F1bf146cfee24e0b140524f4cc44fdf0b.png)



[![9.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2F386486b61c0368168a983087aee9cfee.png)


好了，我们已经获得了在Linux容器里运行.NET。

## 第四步：创建Docker镜像

接下来的步骤是将我们的.NET网络服务打包进一个Docker容器中（在Mac上创建的）。
首先，为了构建一个可部署的.NET程序包先来运行。

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F3cabbbd4e5969da726fd5eb5e7c559be.png)


这会将偶有依赖复制到发布路径，除了.NET Core框架本身。输出的结果将会包含testapp.dll文件，这将会通过执行donet testapp.dll产生。事实上，有一种方法来进行“本地”部署，其中包括了.NET框架和目标系统的二进制可执行文件，但我们在这里并没有这么做。



然后构建容器（docker build -t testapp .）并运行（docker run -it -p 5000:5000 testapp）

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F4e21505ba5e14200ba01b048b65200f6.png)


看起来他正常工作了！唯一出现的问题是该页面服务仅仅监听localhost:5000，所以它不响应来自外部的请求。我们需要通过调节Program.cs中的该*:5000选项作为监听地址

[![12.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Faed9127825ec1be888f2cbcd5fbd330c.png)


再一次运行（ publish > docker build > docker run）进程

[![13.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Faa389e437f3efa9adc9341cff2046140.png)



[![14.png](data:image/svg+xml;utf8,)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2F71a329d32c756f1f8732ecdc9772625d.png)


成功！

## 第五步：在AWS上运行

在这之前运行.NET服务的时候的一个烦恼是，你必须将他们部署为Windows服务的一些共享主机。这介绍了各种问题例如缺乏隔离，缺乏独立检测以及繁琐的部署脚本等。当在云端运行的时候，另一种可供选择的方案是为每个服务启动一台单独的虚拟机（VM），但这会造成资源的极度浪费（特别是当其运行在Windows宿主机上的时候）并额外的让部署时间加长。
容器服务似乎提供了一个完美的解决方案：你有主机集群，在那里你部署的每一个服务会作为一个单独的容器。当然，只有真正适用于Linux的，现在随着.NET Core开启了使用.NET这种策略的服务。
[Amazon EC2 Container Service（ECS）](https://link.juejin.im/?target=https%3A%2F%2Faws.amazon.com%2Fecs%2F)
我们照着ECS的入门指导步骤，这会帮助我们安装AWS命令行，创建AWS Docker仓库，并推送我们的本地镜像到AWS仓库中。

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F3f6bc381d53faff23642f4ba833dab59.png)


为了运行一个容器，你可以把它描述为Task Definition，这其中包括了端口映射以及资源限制（内存、CPU等等）。

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F7912d19a6c7b2ed97a0bd18fc8ca37d1.png)


并创建一个集群来托管容器。

[查看图片](https://link.juejin.im/?target=http%3A%2F%2Fdockerone.com%2Fuploads%2Farticle%2F20160608%2F771aa4c94b7b35816b85acdedc17a040.png)


一旦集群启动，我们可以运行一个新的容器任务将其放置到集群中：

[![18.png](https://user-gold-cdn.xitu.io/2016/11/29/def811f345e904fb9b9bdd1880152b92.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2Fdef811f345e904fb9b9bdd1880152b92.png)


它启动真的很快，而且我们已经得到了我们的在云端运行的Web服务！

[![19.png](https://user-gold-cdn.xitu.io/2016/11/29/27e42f4f5624da096039c563aac008b5.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)](https://link.juejin.im/?target=https%3A%2F%2Fuser-gold-cdn.xitu.io%2F2016%2F11%2F29%2F27e42f4f5624da096039c563aac008b5.png)


这只是基本的用法，但是这时候你就已经有了正在运行的多个实例任务，并包含了负载均衡，自动重启失败实例，零停机时间部署等等这些状态。

真的可以确切地感觉到这些所有的零零散散的东西聚集到一起就是为了.NET和C#可以提供一个现代的跨平台的开发平台。我真的很高兴能继续前进，然后可以在TRAFI里可以应用这样的新的工作流（.NET Core Docker AWS 容器服务）于生产环境。
[这里是规划](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Faspnet%2FHome%2Fwiki%2FRoadmap)

请记住，大多数的信息是很新的，并且在去年一年中有很多事情在开发的时候发生了变化，包括命名（例如ASP.NET 5到 ASP.NET Core，并且打破了API的变化。不过它现在终于变得稳定了，但目前来看仍然有很多过时了的文档和教程。