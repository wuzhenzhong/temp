# npm总结（二）

0.3272018.10.13 20:39:40字数 2265阅读 233

> 本文参考：
>
> - [【原】npm 常用命令详解](http://www.cnblogs.com/PeunZhang/p/5553574.html#npm)
> - [从0开始发布一个无依赖、高质量的npm包](https://github.com/simbawus/blog/issues/12)
> - [Yarn 官网](https://yarnpkg.com/zh-Hans/)

------

上一篇文章 [npm总结（一）](https://www.jianshu.com/p/921e0b89909b)中介绍了`npm`的一些基本使用和工作原理相关的内容，本文将在此基础上，介绍`npm`在实际应用中的一些命令使用和其他高级使用。

### 1. npm 常用命令详解

#### 1.1 npm init 引导输出一个`package.json`文件



![img](https://upload-images.jianshu.io/upload_images/6083825-377c1f3d4b40fdc4.png?imageMogr2/auto-orient/strip|imageView2/2/w/768/format/webp)

npm init 命令语法

1. 语法介绍：调用脚本输出一个初始化的`package.json`文件
2. 命令别名：`create`, `innit`
3. 参数介绍：

- `--force`：简写`-f`，跳过引导交互，快速生成默认的`package.json`文件
- `--yes`：简写`-y`，同上
- `--scope`：同`npm init`，与无参数时相同，交互生成`package.json`文件

#### 1.2 npm install 安装模块

在 [npm总结（一）](https://www.jianshu.com/p/921e0b89909b)中，已经对其做了详细的介绍和使用，那么下面就再总结一下该命令的使用语法：



![img](https://upload-images.jianshu.io/upload_images/6083825-8dafd327c58ac9b0.png?imageMogr2/auto-orient/strip|imageView2/2/w/899/format/webp)

npm install 命令语法

1. 语法介绍：

- `npm install (with no args, in package dir)`：直接根据应用中的`package.json`或`package.lock.json`文件来安装项目或模块需要的依赖
- `npm install [<@scope>/]<pkg>`：安装已发布到`npm`仓库的包名，默认下载`latest`最新发布版
- `npm install [<@scope>/]<pkg>@<tag>`：安装通过`tag`标记获取到的版本
- `npm install [<@scope>/]<pkg>@<version>`：安装指定版本的包
- `npm install [<@scope>/]<pkg>@<version range>`：安装通过`semver`规范匹配到的版本
- `npm install <folder>`：安装本地包
- `npm install <tarball file>`：安装`gzip`压缩文件
- `npm install <tarball url>`：安装可下载到`gzip`压缩包的链接
- `npm install <git url>`：安装通过`git`链接获取到的包

1. 命令别名：`i`, `add`
2. 参数介绍：

- 无参数：同`--save-prod`
- `--global`：简写`-g`，安装全局依赖
- `--save-prod`：同`-save`，简写`-S`，将依赖信息加入到`dependencies`（生产环境的依赖）
- `--save-dev`：简写`-D`，将依赖信息加入到`devDependencies`（开发环境的依赖）
- `--save-optional`：简写`-O`，将依赖信息加入到`optionalDependencies`（可选阶段的依赖）
- `--save-exact`：简写`-E`，精确安装依赖的版本，依赖信息将加入到`dependencies`
- `--no-save`：只添加依赖，依赖信息不写入`package.json`

#### 1.3 npm uninstall 卸载模块



![img](https://upload-images.jianshu.io/upload_images/6083825-05ea7e460a594fca.png?imageMogr2/auto-orient/strip|imageView2/2/w/760/format/webp)

npm uninstall 命令语法

1. 语法介绍：卸载指定的依赖
2. 命令别名：`un`, `unlink`, `remove`, `rm`, `r`
3. 参数介绍：与 `npm install`类似

#### 1.4 npm update 更新模块



![img](https://upload-images.jianshu.io/upload_images/6083825-0513e546fa967cb8.png?imageMogr2/auto-orient/strip|imageView2/2/w/748/format/webp)

npm update 命令语法

1. 语法介绍：更新指定的依赖
2. 命令别名：`up`, `upgrade`

#### 1.5 npm outdated 检查模块是否过时



![img](https://upload-images.jianshu.io/upload_images/6083825-ae9794f46aecc2f1.png?imageMogr2/auto-orient/strip|imageView2/2/w/842/format/webp)

npm outdated 命令语法

语法介绍：检查依赖是否已经过时，并列出所有已经过时的包

#### 1.6 npm ls 查看安装的模块



![img](https://upload-images.jianshu.io/upload_images/6083825-fe469af637cfa89c.png?imageMogr2/auto-orient/strip|imageView2/2/w/767/format/webp)

npm ls 命令语法

1. 语法介绍：列出安装的所有依赖
2. 命令别名：`list`, `la`, `ll`
3. 参数介绍：

- `--global`：简写`-g`，查看全局安装的依赖
- `--depth`：直接列出依赖会列出包括其子依赖，输出太厄长了，使用`--depth=0`控制，如下图所示：



![img](https://upload-images.jianshu.io/upload_images/6083825-332d12d99e35fa69.png?imageMogr2/auto-orient/strip|imageView2/2/w/767/format/webp)

查看全局安装的所有依赖包

#### 1.7 npm config 管理npm的配置路径



![img](https://upload-images.jianshu.io/upload_images/6083825-81a961dc385a43a9.png?imageMogr2/auto-orient/strip|imageView2/2/w/739/format/webp)

npm config 命令语法

1. 语法介绍：所有`npm`配置参数可在官网查看（[npm config](https://docs.npmjs.com/misc/config)）

- `npm config set <key> <value>`：或`npm set <key> <value>`，设置或修改某项`npm`配置。如通过`npm config set proxy=***`设置代理来解决公司内网无法安装`npm`依赖，通过`npm config set registry="https://registry.npm.taobao.org"`切换`npm`仓库源为淘宝镜像来解决无法访问`npm`源或下载依赖慢的问题
- `npm config get <key>`：或npm get [<key>]，查看某项`npm`配置
- `npm config delete <key>`：删除某项`npm`配置
- `npm config list [--json]`：查看所有`npm`配置
- `npm config edit`：用文本编辑器修改`npm`配置

1. 命令别名：`c`

#### 1.8 npm cache 管理模块缓存



![img](https://upload-images.jianshu.io/upload_images/6083825-1081c556c94a5948.png?imageMogr2/auto-orient/strip|imageView2/2/w/719/format/webp)

npm cache 命令语法

语法介绍：

- `npm cache add`：给一个指定的依赖包添加缓存（该命令主要是`npm`内部使用）
- `npm cache clean`：清除`npm`本地缓存
- `npm cache verify`：验证缓存数据的有效性和完整性，清理垃圾数据

#### 1.9 npm version 查看 npm 版本



![img](https://upload-images.jianshu.io/upload_images/6083825-d16f5c77f85aed4c.png?imageMogr2/auto-orient/strip|imageView2/2/w/720/format/webp)

npm version 命令

命令简写：`npm -v`

#### 1.10 npm help 查看某条命令的详细帮助



![img](https://upload-images.jianshu.io/upload_images/6083825-d4c12ad225ca7cea.png?imageMogr2/auto-orient/strip|imageView2/2/w/1038/format/webp)

npm help 命令语法

语法介绍：查看`npm`或某条命令的详细介绍，如`npm help config`，则会在本地生成一个关于config命令使用的HTML文件并在浏览器打开

#### 1.11 npm root 查看依赖包的安装路径

查看`npm`全局依赖包路径：`npm root -g`

#### 1.12 npm run 执行脚本命令

`npm run`命令可以自动新建一个shell去执行里面的脚本，关于`npm`脚本相关的内容，在 [npm总结（一）](https://www.jianshu.com/p/921e0b89909b)中已有介绍，本文就列举一些常用的脚本命令：

- `npm run start`：简写`npm start`，启动模块
- `npm run build`：生成环境打包部署
- `npm run test`：简写`npm test`或`npm t`，测试模块
- `npm run stop`：简写`npm stop`，停止模块
- `npm run restart`：简写`npm restart`，重启模块

#### 1.13 npm 发布包相关命令

- `npm login`：同`npm adduser`，登录`npm`社区账号，如下图所示：



![img](https://upload-images.jianshu.io/upload_images/6083825-d44b9d61e3b94070.png?imageMogr2/auto-orient/strip|imageView2/2/w/777/format/webp)

npm login 命令使用

- `npm publish`：发布`npm`包



![img](https://upload-images.jianshu.io/upload_images/6083825-1dee0d023a74b8c0.png?imageMogr2/auto-orient/strip|imageView2/2/w/831/format/webp)

npm publish 命令语法

关于如何发布一个`npm`包，可参考文章：[从0开始发布一个无依赖、高质量的npm包](https://github.com/simbawus/blog/issues/12)

### 2. npm 安装依赖过程中的错误解决

#### 2.1 安装时间长导致安装失败

由于国内防火墙原因，有时候下载`npm`包特别慢，还经常下载失败，可以尝试以下方法来解决：

1. 使用`cnpm`替代`npm`，通过`npm`安装：`npm install -g cnpm`，使用时将所有的`npm`命令换为`cnpm`即可
2. 使用`nrm`快速切换`npm`仓库源，通过`npm`安装：`npm install -g nrm`，`nrm`命令使用如下图所示：



![img](https://upload-images.jianshu.io/upload_images/6083825-90e2a70fdc90e426.png?imageMogr2/auto-orient/strip|imageView2/2/w/957/format/webp)

nrm 命令介绍

推荐方式：先使用`nrm test`测试仓库源的延迟，然后使用`nrm use`命令选择延迟最少的使用，`nrm`的作用只是快速切换`npm`仓库源，所以使用时还是用`npm`命令

#### 2.2 其他未知问题

有时候会碰到一些莫名其妙的问题导致安装依赖失败，可尝试升级`node`版本，升级`npm`版本，清除`npm`缓存：`npm cache clean --force`等方法来解决

### 3. Yarn

#### 3.1 为什么使用 yarn

因为早期`npm`有较多的问题，如安装依赖的速度较慢，安装的依赖版本也会不一致而引发一系列的问题，而`yarn`正是为了弥补`npm`的缺陷而出现的。正如官网介绍的其优点：

1. **极其快速**：`Yarn`会缓存它下载的每个包，所以无需重复下载。它还能并行化操作以最大化资源利用率，安装速度之快前所未有
2. **特别安全**：`Yarn`会在每个安装包被执行前校验其完整性
3. **超级可靠**：`Yarn`使用格式详尽而又简洁的`lockfile`文件 和确定性算法来安装依赖，能够保证在一个系统上的运行的安装过程也会以同样的方式运行在其他系统上

#### 3.2 安装

在`windows`上安装`yarn`最简单的方法就是使用`npm`来安装：`npm install -g yarn`，其他方法可参见官网：[安装 | Yarn](https://yarnpkg.com/zh-Hans/docs/install#windows-stable)，下面主要介绍一下`CentOS`系统上的安装（[安装 | Yarn](https://yarnpkg.com/zh-Hans/docs/install#centos-stable)）：

```bash
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo

sudo yum install yarn

yarn --version
```

#### 3.3 使用

`yarn`的使用和`npm`十分相似，大部分命令还是相同的，在官网[从 npm 迁移](https://yarnpkg.com/zh-Hans/docs/migrating-from-npm)中也列出了一些常用命令的比较，如下图所示：



![img](https://upload-images.jianshu.io/upload_images/6083825-73ff954f1176099c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

npm 与 yarn 命令比较

其他的`yarn`命令的使用，在官网也有详细的介绍：[CLI 介绍 | Yarn](https://yarnpkg.com/zh-Hans/docs/cli/)

#### 3.4 小结

关于`yarn`的使用，本文不做详细介绍，因为官网的文档已经很清晰了，在理解了`npm`的基础上，快速阅读下`yarn`的文档即可使用`yarn`作为你的包管理工具。另外，关于`package.json`文件中的字段解释，在`yarn`官网（[package.json | Yarn](https://yarnpkg.com/zh-Hans/docs/package-json)）有很详细的解释，很值得去阅读查阅。

那么使用了`yarn`就可以完全替代`npm`吗，答案是肯定的，虽然官网文档说两者一起使用也是完全可以的，但还是尽量避免这样做。至于选择哪个包管理工具，完全由个人喜好而定，`npm`也更新到`6`版本了，作为竞争者，自然也不会比`yarn`差到哪里去，所以选择一个自己喜欢的工具使用就好。