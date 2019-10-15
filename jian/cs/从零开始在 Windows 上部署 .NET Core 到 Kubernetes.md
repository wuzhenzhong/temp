# 从零开始在 Windows 上部署 .NET Core 到 Kubernetes

阅读 257

收藏 3

2019-01-06

原文链接：[siegrain.wang](https://link.juejin.im/?target=https%3A%2F%2Fsiegrain.wang%2Fkubernetes%2Fwindows-net-core-kubernetes%2F)

[在实时音视频中应用深度学习，你需要了解这些juejin.im](https://juejin.im/post/5d9da1446fb9a04df26c2145)

本章节所有代码已上传至：[github.com/Seanwong933…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FSeanwong933%2F.NET-Core-on-Kubernetes)

文末附有本人遇到过的 Docker 和 k8s 的故障排除。

**本文目标：带领大家在 Kubernetes 上部署一个 .NET Core Api 的单节点集群。**

后续文章会帮助大家继续深入。

## 安装 Kubernetes

以下所有命令都要在管理员模式下执行。

1. 下载安装最新版 Docker for Windows

   hub.docker.com/editions/co…

    

   然后跑一下

   ```
   docker ps
   ```

   看安装成功没有，没有就重启一下你的命令行工具或电脑，环境变量没起作用。

2. 设置国内镜像 `https://registry.docker-cn.com` ![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185f5406d7b8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

3. 下载 docker 镜像并加载

   这一步是为了把 Kubernetes 所需要的依赖镜像先下载到本地，相当于一个加速服务，不过根据我的个人经验，如果你不用这个加速的话基本没可能下得下来，即使你有代理。

   ```
   git clone https://github.com/AliyunContainerService/k8s-for-docker-desktop.git
   cd k8s-for-docker-desktop
   .\load_images.ps1
   ```

4. 打开 docker 开启 Kubernetes，等待安装完成

   

5. 在 Powershell 中安装 kubectl

   kubectl 简单来说，就是一个操作 Kubernetes 的工具。

   ```
   Install-Script -Name install-kubectl -Scope CurrentUser -Force
   ```

   然后在类似这样的位置中

    

   ```
   E:\文档\WindowsPowerShell\Scripts
   ```

    

   找到脚本并执行

   ```
   install-kubectl.ps1
   ```

   可能会报错，不管，不影响使用。

   或通过 Chocolatey 来安装（推荐）

   Chocolatey 是一个包管理器，没有的同学自己装一下。

   ```
   choco install kubernetes-cli
   kubectl version
   ```

   如果说找不到，就跑一下

   ```
   choco search kubernetes-cli
   ```

   和

   ```
   choco list kubernetes-cli
   ```

   ，有时候会抽风。 进入你的用户目录：

   ```
   cd C:\users\yourusername
   ```

   创建.kube目录：`mkdir .kube`

   进入：`cd .kube`

   添加配置文件：`New-Item config -type file`

6. 此时可以跑一下

   ```
   kubectl get nodes
   ```

   、

   ```
   kubectl get services
   ```

   检查安装效果

   ![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185f53f774c7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 三大组件：POD & Service & Deployment

在正式开始之前，先粗略一下里面关键组件，如果你看完还是啥也不明白，可以配合这篇文章一起阅读：[十分钟带你理解Kubernetes核心概念](https://link.juejin.im/?target=http%3A%2F%2Fdockone.io%2Farticle%2F932)

![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185f541165e7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- Pod

  Kubernetes 中的最小单元，一个 Pod 里面可以放很多个应用，支持多容器在一个 Pod 中通过进程进行通信

- Service 服务

  Pod 的对外入口，需要这个才能在外部访问 Pod

- Deployment 部署

  表示用户对 Kubernetes 的一次更新操作，通过部署模板将 Pod 跟 Service 绑定

**粗暴理解，用 Deployment 可以部署 Pod，然后通过 Service 来暴露对 Pod 的访问。**

## Service 的三种类型

1. ClusterIP

   一个集群内部服务，默认情况外部无法访问，需要通过 kubectl 的代理命令转发访问。

2. NodePort

   在所有节点上开放一个特定端口，将该端口的流量转发到对应的服务，是开发时经常使用的暴露 Pod 的方法，没有代理那么麻烦。

3. LoadBalancer

   Kubernetes 的负载均衡，需要把你的负载均衡器（你集群的负载均衡器或云服务商的）与它关联起来，就可以帮你转发流量了。

## 安装 Kubernetes Dashboard

顾名思义仪表盘嘛，用来展示 Kubernetes 的各方面数据，也可以做一些比较简单的操作。

找回刚才的 k8s-for-docker-desktop 目录，我们要用里面的配置文件来安装。

```
kubectl create -f kubernetes-dashboard.yaml
```

在配置文件中，有一个namespace的配置，指的是你服务的命名空间。

![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185fbbed4f49?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

然后我们来用命名空间练练手：

用`kubectl get namespace`获取你所有的命名空间

用`kubectl get deploy -n kube-system`命令获取你这个空间下的部署

用`kubectl get service -n kube-system`获取服务的内部地址

![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185fc3a70f10?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

上面的这些示例命令应该不会有什么问题，接下来输入`kubectl proxy`，将内部地址通过代理转发出来，就可以看到dashboard了。

如果你没有做什么特殊操作，以下URL应该就能够访问到它：[http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login](https://link.juejin.im/?target=http%3A%2F%2Flocalhost%3A8001%2Fapi%2Fv1%2Fnamespaces%2Fkube-system%2Fservices%2Fhttps%3Akubernetes-dashboard%3A%2Fproxy%2F%23!%2Flogin)

![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185fdd881a6f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

然后我们的dashboard就跑在其中一个pod下：

![img](https://user-gold-cdn.xitu.io/2019/1/6/1682185fe46d2c12?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

这个dashboard的访问地址很长，所以接下来要把它配置成NodePort转发出来。

把刚才的配置文件搞下来，找到最后一坨，把带注释的两句加进去：

```
# ------------------- Dashboard Service ------------------- #

kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  type: NodePort # 指定类型
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 30003 # 指定对外端口
  selector:
    k8s-app: kubernetes-dashboard
```



删除`deploy`和`service`，再创建一下

```
kubectl delete deploy kubernetes-dashboard -n kube-system
kubectl delete svc kubernetes-dashboard -n kube-system

kubectl create -f .\kubernetes.dashboard.yaml
```

这时会提示你有些东西已经存在了，不管就是了。

然后通过`kubectl get svc -n kube-system`可以看到你的新服务已经部署上去了：

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="508" height="49"></svg>)

这个时候再`kubectl proxy`访问一下 [https://localhost:30003](https://link.juejin.im/?target=https%3A%2F%2F127.0.0.1%3A30003%2F)

这里要注意的是，默认分配的443端口也就是默认https，windows下访问会被栏掉而mac下不会，不过这不是重点，学会改`NodePort`就行了。

### 额外参考

[kubectl命令技巧大全](https://link.juejin.im/?target=https%3A%2F%2Fjimmysong.io%2Fposts%2Fkubectl-cheatsheet%2F)

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="472" height="143"></svg>)

## 初始化一个 .NET Core API 并 push 到 docker hub

接下来我们放一下Kubernetes，需要先学习一下如何部署一个.NET Core应用到Docker上，并提交到 Docker Hub，毕竟学会了走才能跑。

1. 创建一个web api

   ```
   dotnet new webapi -n k8s-demo
   ```

2. 修改一个action方便看效果，然后本地测试一下看能不能跑通

   ```
   // GET api/values/5
   [HttpGet("{id}")]
   public ActionResult<string> Get(int id)
   {
       return $"你输入的是：{id}";
   }
   ```

   

3. 创建Dockerfile

   ```
   FROM microsoft/dotnet:sdk AS build-env
   WORKDIR /app
   EXPOSE 80
   
   # Copy csproj and restore as distinct layers
   COPY *.csproj ./
   RUN dotnet restore
   
   # Copy everything else and build
   COPY . ./
   RUN dotnet publish -c Release -o out
   
   # Build runtime image
   FROM microsoft/dotnet:aspnetcore-runtime
   WORKDIR /app
   COPY --from=build-env /app/out .
   ENTRYPOINT ["dotnet", "k8s-demo.dll"]   # 注意这里改一下dll名称
   ```

4. 在本地运行Docker镜像，注意这里的

   ```
   dockerusername
   ```

   换成你的docker用户名。

   ```
   docker run -d -p 8080:80 --name k8s-demo dockerusername/k8s-demo .
   ```

5. 查看效果

   ```
   docker ps
   ```

   

    

   再进入对应的页面看一下：

   http://127.0.0.1:8080/api/values/5

6. 登录 docker 以 push 你的镜像，或者你本地登录过直接

   ```
   docker login
   ```

   即可

   ```
   docker login --username dockerusername
   ```

7. 推送镜像

   ```
   docker push dockerusername/k8s-demo
   ```

## 把 .Net Core API 部署到 Kubernetes

### 部署文件

还记得之前说的三大组件吗？这个文件就是用来部署 Kubernetes 集群的。

同时，我们这里用的是一个`Yaml`格式的文件，`Yaml`是一个可以和`Json`无缝转换的配置文件类型，字符串不需要加引号，可以自动被识别，字典前带`-`号，注意`Yaml`是通过缩进来管理配置节点的子父级的，所以不能随意缩进。

以下是一个标准的 .net core api 的 kubernetes 部署文件，部署`Deployment`跟`Service`两个组件。

```
---

kind: Deployment   # 组件类型
apiVersion: apps/v1
metadata:
  name: hello-api
  namespace: netcore  # 可指定，不指定时使用默认命名空间
  labels:
    name: hello-api
spec:
  replicas: 2 # 部署两份叫 hello-api 的容器
  selector:
    matchLabels:
      name: hello-api
  template:
    metadata:
      labels:
        name: hello-api
    spec:
      containers:
        - name: hello-api
          image: yourdockername/k8s-demo  # docker hub 中的镜像名称，修改为你的镜像名称
          ports: 
          - containerPort: 80
          imagePullPolicy: Always

---

kind: Service
apiVersion: v1
metadata:
  name: hello-api
  namespace: netcore
spec: 
  type: NodePort
  ports: 
    - port: 80
      targetPort: 80
  selector:
    name: hello-api # 对应要映射的Pod
```

我们将它命名为`deploy.yaml`，记住修改`yourdockername/k8s-demo`为你的镜像名称。

然后执行部署命令。

```
kubectl create -f deploy.yaml
```

如果这里指定了不存在的`namespace`要用`kubectl create namespace`命令创建。

查看URL，我的`namespace`就叫`netcore`

```
kubectl get svc -n netcore
```

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="587" height="56"></svg>)

打开这个页面试一下：[http://localhost:31295/api/values/5](https://link.juejin.im/?target=http%3A%2F%2Flocalhost%3A31295%2Fapi%2Fvalues%2F5)

这个时候进入dashboard，可以查看.net core的日志甚至可以执行命令。

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1240" height="615"></svg>)

### 额外参考

通过 `kubectl explain path.to.nodes` 查看各个配置文件节点的解释，比如你要看`metadata`节点下的`name`节点的解释，就执行`kubectl explain metadata.name`

到这里就恭喜你已经成功部署了一个 Kubernetes 的单节点集群了，下面我们继续深入介绍一下。

## Kubernetes 集群高级概念

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="906" height="587"></svg>)

图左边是 Master 控制节点，右边是 Worker 工作节点。

**Master部分**

从上到下依次来说

- kubectl → authentication → REST(apiserver)

  这一个流程是用户交互、认证、然后调用 Kubernetes 各种接口的一个流程（kubectl xxx）

- ETCD

  右下角的ETCD是一个分布式数据库，负责保存整个 Kubernetes 集群的状态

- scheduler

  负责调度，比如创建一个 Pod，找一个负载低一点的 CPU 存储，而 ETCD 是只知道存储，但不知道存哪儿好，所以需要 Scheduler 来调度一下

- controller manager

  负责维护集群的状态，比如故障检测、自动扩展、滚动更新等

**Worker部分**

- Node

  表示集群中的一个主机单元，可以是物理机也可以是虚拟机

- kubelet

  kubernetes 与 docker 的交互组件，负责容器的各种操作、生命周期等

- Proxy

  Proxy 负责网络转发

- Pod

  这个不用多说，就是你创建的一个个容器

而以上的这些组件，其实也都是 docker 的镜像，跑一下`docker images`就能看见各个组件的镜像。

还有些图上没有的，比如 container runtime，负责镜像管理和容器的真正运行。

## Kubernetes 调度过程

下图展现了一个完成的调度过程，一步一步来说。

![img](https://user-gold-cdn.xitu.io/2019/1/6/16821860ce417621?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. kubectl 将创建 Deployment 资源这个命令发送到 API Server ，会先将这个命令保存到ETCD中，验证成功后直接返回，因为这里采用的是异步调用。
2. 上一步验证完成后，Controller Manager 收到通知
3. Controller Manager 让 Deployment Controller 创建一个 ReplicaSet（副本）
4. ReplicaSet Controller 收到通知，创建 Pod
5. Scheduler 收到通知，将 Pod 分配到 Worker 中某一个合适的 Node 上
6. 然后 Kubelet 告诉 Docker 我要（根据 Pod）创建容器了，它的镜像是什么，版本是什么
7. 启动 Containers

可以看到整个 Kubernetes 调度基本全都是在 Master 节点在做的，所以 Master 节点绝对不能挂。

下面大概介绍一下，一套 Kubernetes 高可用集群大概是什么样的：

后续文章会继续教大家深入 Kubernetes。

## 额外章节：故障排除

### Unable to create: 已停止该运行的命令，因为首选项变量“ErrorActionPreference”或通用参数设置为 Stop: 对象已存在。

当你的 Docker 运行在 linux 容器模式下就可能会报这个错误。

```
Unable to create: 已停止该运行的命令，因为首选项变量“ErrorActionPreference”或通用参数设置为 Stop: 对象已存在。


在 Docker.Core.Pipe.NamedPipeClient.Send(String action, Object[] parameters) 位置 C:\workspaces\stable-18.09.x\src\github.com\docker\pinata\win\src\Docker.Core\pipe\NamedPipeClient.cs:行号 36
在 Docker.Actions.DoStart(SynchronizationContext syncCtx, Boolean showWelcomeWindow, Boolean executeAfterStartCleanup) 位置 C:\workspaces\stable-18.09.x\src\github.com\docker\pinata\win\src\Docker.Windows\Actions.cs:行号 92
在 Docker.Actions.<>c__DisplayClass19_0.<Start>b__0() 位置 C:\workspaces\stable-18.09.x\src\github.com\docker\pinata\win\src\Docker.Windows\Actions.cs:行号 74
在 Docker.WPF.TaskQueue.<>c__DisplayClass19_0.<.ctor>b__1() 位置 C:\workspaces\stable-18.09.x\src\github.com\docker\pinata\win\src\Docker.WPF\TaskQueue.cs:行号 59
```



解决方案：执行以下命令

```
MOFCOMP %SYSTEMROOT%\System32\WindowsVirtualization.V2.mof
```



或删掉、卸载、禁用所有 在`设备管理器 -> 网络适配器`下的 Hyper-V 虚拟网络适配器。

参考地址：

[github.com/docker/for-…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fdocker%2Ffor-win%2Fissues%2F1538)
[community.spiceworks.com/how_to/1223…](https://link.juejin.im/?target=https%3A%2F%2Fcommunity.spiceworks.com%2Fhow_to%2F122307-fix-error-managing-hyper-v-server-2012-r2-from-windows-10)

### 执行 docker ps 等命令时弹出：`error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.26/containers/json: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.`

报这个有很多原因，有可能是你混装了`Docker for Windows`和`Docker Toolbox`，并混用了 Windows 容器模式跟 Linux 容器模式，用以下方式应该可以解决：

1. 卸载 Docker for Windows 和 Docker Toolbox
2. 关闭 Hyper-V 重启
3. 设备管理器 -> 网络适配器 中卸载所有 Hyper-V 适配器跟 VirtualBox 适配器
4. 删除所有 AppData 等用户文件夹下的 Docker 相关文件
5. 清除所有关于 Docker 的环境变量（重要），然后开启 Hyper-V -> 重启 -> 重装Docker for Windows