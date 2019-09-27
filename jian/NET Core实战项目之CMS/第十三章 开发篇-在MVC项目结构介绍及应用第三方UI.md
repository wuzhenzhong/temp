# .NET Core实战项目之CMS 第十三章 开发篇-在MVC项目结构介绍及应用第三方UI

2019.01.06 19:59:26字数 2575阅读 53

作为后端开发的我来说，前端表示真心玩不转，你如果让我微调一个位置的样式的话还行，但是让我写一个很漂亮的后台的话，真心做不到，所以我一般会选择套用一些开源UI模板来进行系统UI的设计。那如何套用呢？今天就以我们系列实战教程中的CMS系统为例来应用第三方的后台模板LayuiCMS2.0为例来进行实战演练吧！

> 本文已收录至《[.NET Core实战项目之CMS 第一章 入门篇-开篇及总体规划](https://www.cnblogs.com/yilezhu/p/9977862.html)》
> 作者：依乐祝
> 首发地址 “DotNetCore实战”公众号
> 原文地址：https://www.cnblogs.com/yilezhu/p/10210732.html

## 写在前面

这里先对从一开始就开始追这个ASP.NET Core实战项目之CMS系列教程的读者朋友说声抱歉。说因为太忙很久没更新了，这都是托词！其实我是想趁着元旦假期把后台功能全部写完以后再来一点点的再分享出来的，奈何前端知识的软肋彻底爆发，再加上周末陪小孩因此只完成了百分五十左右的开发工作，再者群里很多小伙伴也已经在崔更了，所以摒弃之前的策略，改成边分享边开发的策略。接下来还是尽量保持一星期三篇左右的进度来分享，写文章真的比写代码要更费神，希望大家能够多多支持，多多推荐！你的推荐也是我继续分享的动力！当然祝福大家“元旦后第一天正式上班开开心心，快快乐乐”。

## 实战

### ASP.NET Core MVC项目结构介绍

在开始之前先让我们大致了解下一个新创建的ASP.NET Core MVC的项目结构，只有了解了项目结构后，我们才能得心应手的进行相关的操作！同时由于`Czar.Cms.Admin`项目中我已经应用过了模板，因此我们以`Czar.Cms.Site`这个还没有动过手术的前端项目为例来进行讲解。

1. 新创建的一个空的ASP.NET Core MVC的项目结构如下所示，我们只介绍圈起来的八个部分：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-55a376a3649523f5.png?imageMogr2/auto-orient/strip|imageView2/2/w/248/format/webp)

   1546417896621

2. wwwroot部分放的内容都是前端的内容，如css，js，image等等。如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-34a7eeec1b34bedc.png?imageMogr2/auto-orient/strip|imageView2/2/w/320/format/webp)

   1546418188521

   ASP.NET Core MVC项目为我们生成了一套默认的样式，如上图红圈圈起来的部分就是这套默认的样式（下面再一步一步的替换它），我们按如下图所示的操作选择这个项目，然后右键-》查看-》在浏览器中查看，

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-f2a4c5edc10abcfe.png?imageMogr2/auto-orient/strip|imageView2/2/w/980/format/webp)

   1546418317631

   看到如下的默认界面（别急，我们接下来再替换它）

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-bc08cd479bed14c3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1154/format/webp)

   1546418460178

3. 依赖项：顾名思义就是项目所需要以来的第三方组件，比如我们的`Czar.Cms.Site`项目需要依赖仓储层的项目，比如我们用到了第三方的Autofac组件等等，如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-bad6af45ee9de567.png?imageMogr2/auto-orient/strip|imageView2/2/w/313/format/webp)

   1546418752015

4. Controllers：MVC架构中的C层即控制器层，用到Asp.Net MVC的对这个控制器应该不陌生吧！这个 文件夹下包含负责处理用户输入和响应的控制器类。另外要求所有控制器的名称必须以 "Controller" 结尾。如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-5920359a69ff1e4b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1115/format/webp)

   1546419006648

5. Models：MVC架构中的M层即实体层，这个大伙应该都熟悉吧就是实体对象，这里如果我再截图的话感觉就是在侮辱大伙的智商，所以~~~~

6. Views：MVC架构中的V层即视图层，用来在浏览器中显示的具体界面。如下图所示，这里跟Controller层进行对应，如上图标注的HomeController中的Index就对应Views文件夹下Home文件夹下面的Index.cshtml文件：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-87307791363194fc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

   1546419258081

   我们尝试把Index.cshtml中的内容改为`Welcome 依乐祝！`，然后再浏览器中打开查看一下，可以看到页面的内容已经发生了变化

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-e9b6a159315199bd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1028/format/webp)

   1546419384040

7. appsettings.json：这个文件是系统配置文件，不知道大家还记得《[.NET Core实战项目之CMS 第三章 入门篇-源码解析配置文件及依赖注入](https://www.cnblogs.com/yilezhu/p/9998021.html)》这篇文章中的内容吗？里面详细介绍了这个文件的加载过程。

8. Program.cs：及系统的启动入口，熟悉C#的童鞋是不是感觉似曾相识，没错，就是一个控制台程序的入口嘛！你是否在想，里面会不会有Main方法呢？哈哈，自己打开看下吧！

9. Startup.cs：启动配置文件，第三篇也说过了，其实这个文件会被转换成`IStartUp`然后再进行注入的！不清楚的可以去看第三篇，有提到！

### Views结构介绍

关于Views的接哦古，感觉还是有必要提一下，不知道大伙有没有注意到我们上面打开的Index.cshtml文件，这个里面好像没有html，head,title,body等等标签啊，但是如果我们再浏览器中右键查看源文件，可以看到如下所示引入了很多的js以及css样式文件啊，这究竟是怎么做到的呢？别急，我们这个小节就来阐述。



![img](https://upload-images.jianshu.io/upload_images/2767091-53f84bb13ba4add5.png?imageMogr2/auto-orient/strip|imageView2/2/w/990/format/webp)

1546420118600

1. Views文件夹下面有一个特殊的文件夹即`Shared`文件夹以及特殊的文件，以`_`开头的文件。如下图所示红色圈圈圈起来的，

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-3390fa1e6cb25ae6.png?imageMogr2/auto-orient/strip|imageView2/2/w/289/format/webp)

   1546420212125

2. Shared文件夹下面就是定义一些公共部分的模板，就以MVC默认模板为例，如定义公共的头部菜单部分，或者公共的底部部分，我们以`Shared\_Layout.cshtml`为例进行讲解,如下图所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-5ae6c83d58703766.png?imageMogr2/auto-orient/strip|imageView2/2/w/848/format/webp)

   1546420544607

   这个文件定义了一个标准的html5的模板，包含头部，导航部分，正文有差异的不放呢，底部，甚至可以根据环境变量加载不同的内容。这里留个问题，那我们前面看的Home/Index.cshtml用的是哪个样式呢？

3. 再看看跟Shared文件夹平级的文件`_ViewStart.cshtml` 可以看到如下内容：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-8f5ba9de4b5d0ee3.png?imageMogr2/auto-orient/strip|imageView2/2/w/790/format/webp)

   1546420787331

   这个文件就是用来定义全局的模板引用规则的，如上图，这里给所有的视图默认应用了`_Layout`的模板，也就是2中流的思考题的答案，即应用了`Shared/_Layout.cshtml`这个模板的样式。

### 应用第三方UI模板

了解了上面的结构后，我们知道，如果想应用第三方的UI，那么我们得把默认生成的wwwroot中的内容替换成我们使用的第三方模板，然后按照第三方UI模板的格式，在`Shared\_Layout.cshtml`中拷贝公共的模板，然后再把变化的部分放到对应的页面即可。明白了原理，在开始动手覆盖是不是感觉得心应手了呢？还不赶紧开始吧！毕竟是实战，如果不实战一番感觉对不起这个名字，So,我就以LayuiCms2.0为例来给大伙实战一番。刚好我们的CMS后台就是用的这个模板。

1. 想引用第三方模板之前是不是得先把模板下下来呢？如果你也想用LayuiCms2.0，可以[点这里下载](https://gitee.com/layuicms/layuicms2.0)

2. 解压后把里面的css,images,js等文件拷贝到`wwwroot`目录里面，当前拷贝之前还是建议你先把这个目录下面的所有文件都清理掉。由于这个Layuicms依赖Layui,所以还需要把layui文件夹拷贝过去。这个每个UI模板不一样需要拷贝的内容不尽相同。

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-0d13da5f01456d32.png?imageMogr2/auto-orient/strip|imageView2/2/w/630/format/webp)

   1546427936460

拷贝后的目录结构如下所示：这里我把json文件也拷贝过去了，后期再把对应的json文件替换掉！先用静态数据演示。



![img](https://upload-images.jianshu.io/upload_images/2767091-0f52daa298bb5837.png?imageMogr2/auto-orient/strip|imageView2/2/w/203/format/webp)

1546427998730

1. 替换Shared对应的模板，把整个文件简单粗暴的拷贝过去，然后做相应的替换即可，对于变化的部分应用`@RenderBody()` ,替换后的内容如下所示：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-0f9056493392cae5.png?imageMogr2/auto-orient/strip|imageView2/2/w/817/format/webp)

   1546428149213

2. 把变化的内容放到具体的页面中吧，这里我只展示一个main里面的内容：

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-0d17ac229a39455e.png?imageMogr2/auto-orient/strip|imageView2/2/w/789/format/webp)

   1546428293861

3. 到此结束，其他的模板替换方案类似。

### 效果展示

这里话不多说，给大家展示下效果吧：

主页



![img](https://upload-images.jianshu.io/upload_images/2767091-1f9c5f297ca0f31f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428396237

角色管理：



![img](https://upload-images.jianshu.io/upload_images/2767091-5012e4ca3a129d4c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428441722

角色编辑：



![img](https://upload-images.jianshu.io/upload_images/2767091-73093f1bbe42f08e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428474235

用户管理：



![img](https://upload-images.jianshu.io/upload_images/2767091-2f13f8543eb0778e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428506860

用户管理编辑：



![img](https://upload-images.jianshu.io/upload_images/2767091-da4a0c319202476f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428532563

用户管理列表页，锁定用户：



![img](https://upload-images.jianshu.io/upload_images/2767091-a0c30576f9cf569a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428568258

锁定界面：



![img](https://upload-images.jianshu.io/upload_images/2767091-0673fdbd982c64b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1546428593869

目前只实现了这些功能其他后续再展示。

## 开源地址

这个系列教程的源码我会开放在GitHub以及码云上，有兴趣的朋友可以下载查看！觉得不错的欢迎Star

GitHub：https://github.com/yilezhu/Czar.Cms

码云：https://gitee.com/yilezhu/Czar.Cms





如果你觉得这个系列对您有所帮助的话，欢迎以各种方式进行赞助，当然给个Star支持下也是可以滴！另外一种最简单粗暴的方式就是下面这种直接关注我们的公众号了：



![img](https://upload-images.jianshu.io/upload_images/2767091-238c5fa29e65e38b.png?imageMogr2/auto-orient/strip|imageView2/2/w/258/format/webp)

img

## 总结

今天我给大家讲解了ASP.NET Core MVC项目的结构，并详细阐述了View层的模板嵌套原理。接着带着大家一步一步的操作了一遍如何应用第三方UI模板。当然源码也已经同步更新到GitHub上了，有兴趣的小伙伴可以下载参考！下一篇我会带着大家结合这个模板，来讲解如何实现角色的增删改查！以及项目各个结构之间的协调工作！敬请期待吧！放心，下篇周内就会赶出来！另外如果你有任何问题可以下方留言或者加群637326624跟大伙进行沟通。