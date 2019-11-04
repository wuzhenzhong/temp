# jupyter安装步骤

[![img](https://upload.jianshu.io/users/upload_avatars/11983030/0490b954-f407-4d5c-8450-ecde6c08468a?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/054ab278d5d9)

[程序员界李教授](https://www.jianshu.com/u/054ab278d5d9)关注

0.1442018.09.24 20:23:37字数 1,215阅读 5,647

一、jupyter notebook是什么

官网的介绍是：Jupyter Notebook是一个Web应用程序，允许您创建和共享包含实时代码，方程，可视化和说明文本的文档。 用途包括：数据清理和转换，数值模拟，统计建模，机器学习等等。

简单的介绍就是：Jupyter Notebook是Ipython的升级版，而Ipython可以说是一个加强版的交互式 Shell，也就是说，它比在terminal里运行python会更方便，界面更友好，功能也更强大。怎么强大法，往下看就知道了。

二、jupyter notebook的安装和打开

安装非常简单，只需要在终端输入：

[plain] view plain copy

pip install jupyter  

打开jupyter notebook 也只需要在终端输入：

[plain] view plain copy

jupyter notebook  

运行上面的命令之后，你将看到类似下面这样的输出：





![img](https://upload-images.jianshu.io/upload_images/11983030-9ec80de786555ea9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1062/format/webp)

如上图，它打开了一个端口，并且会在你的浏览器中打开这个页面，主目录是图中的那个directory(可能第一次打开没有这个目录)。

三、使用

1、打开一个新文档





![img](https://upload-images.jianshu.io/upload_images/11983030-5a41deac86566d03.png?imageMogr2/auto-orient/strip|imageView2/2/w/162/format/webp)

在主页面的右上角点new即可新建一个你想要的文件类型。

如上图，jupyter也可以打开一个terminal，还可以作为一个text文本编辑器，功能明显是比terminal强大了。

下面的Notebooks类型除了python 也是可以加入其他类型的文档的，具体方法百度一下就好。

2、python编辑器介绍

点击python2后会出现一下界面：





![img](https://upload-images.jianshu.io/upload_images/11983030-f6569e216dc369b8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1144/format/webp)

稍微介绍一下notebook 界面的组成部分1）notebook 的名称2）主工具栏提供了保存、导出、重载 notebook，以及重启内核等选项3）快捷键4）notebook 编辑区

最下面的哪个 In [ ]: 的框叫做单元格，你可以把你的代码分成一段段的单元格输入，然后可以逐个单元格地运行。注意，这个功能是非常友好的，有时候只修改了中间的一小段代码，又不想全部代码都要重新运行的时候这个功能就非常有用了。另外，单元格是可以改变顺序的。而且可以输出图片和绘图！非常强大吧！

这些只要稍微尝试一下就懂的，下面主要介绍一些常用的技巧

**注意，jupyter notebook 是支持 TAB 键自动补充单词的，再一次展示了其强大友好的一面！

A.修改文档名称

方法一：点上图的Untitled

方法二：点File，再点rename

B.导出文档

步骤：点File，再点Download as





![img](https://upload-images.jianshu.io/upload_images/11983030-1b94dd012b136cac.png?imageMogr2/auto-orient/strip|imageView2/2/w/324/format/webp)

发现里面支持好几种格式的导出，第一个ipynb是notebook的格式，是一种类json的格式保存，其他的建议你们都试一试，你会感到非常惊喜的。

C.保存

Ctrl + S 快捷键的可以保存你的文档的，默认是保存为ipynb，保存在你的主目录下！

D.单元格格式

注意到快捷键栏中有一个code的下拉框，点开发现有几个选项：





![img](https://upload-images.jianshu.io/upload_images/11983030-7c1579317bf67ae5.png?imageMogr2/auto-orient/strip|imageView2/2/w/113/format/webp)

这里介绍一下

Code格式就是正常的python代码格式

Markdown的一个text文档编辑格式，就像在word里编写一样

Heading就是给Markdown的句子设置标题等级，像word的标题一，标题二...

Raw NBConvert 没用过不了解，可以自行百度或者看官网介绍

下面举例说明一下

选择一个空的单元格，code下拉框选择Heading，会出现一个不同类型的 cell：





![img](https://upload-images.jianshu.io/upload_images/11983030-be97a8926e519da2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1151/format/webp)

改变单元格类型时弹出消息中有解释，后面那个单元格以 # 标记开头，意味着这是一个一级标题。如果需要子标题，可以使用以下标记表示：

\# : 一级标题## : 二级标题### : 三级标题...

输入内容后再运行一下（快捷栏里有），会出现类似下面的情况：





![img](https://upload-images.jianshu.io/upload_images/11983030-c53199b7c463d0d5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1141/format/webp)

我一共输入了三级标题，点其中一个，你会发现它的code下拉栏显示是markdown类型

你以后代码里print 的内容都是以markdown的格式显示的。

E.快捷键

常用的快捷键是：

Ctrl + Enter: 执行单元格代码

Shift + Enter: 执行单元格代码并且移动到下一个单元格

Alt + Enter: 执行单元格代码，新建并移动到下一个单元格

这几个快捷键都是非常常用的。

net/gubenpeiyuan/article/details/79252402?utm_source=copy