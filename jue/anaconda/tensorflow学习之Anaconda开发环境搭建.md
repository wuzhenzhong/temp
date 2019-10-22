# tensorflow学习之Anaconda开发环境搭建

0.2272018.08.20 13:38:39字数 1315阅读 2283

tensorflow的开发环境有很多，可以在Docker上搭建，也可以使用Anaconda管理工具搭建，也可以直接在本机中安装tensorflow。在这里为了工具包的方便管理，我选择使用Anaconda搭建。

### 环境搭建

- 下载并安装Anaconda
- 下载并安装tensorflow
- 下载并安装notebook

### 下载Anaconda

#### Anaconda是什么？

Anaconda 是一种Python语言的免费增值开源发行版，用于进行大规模数据处理, 预测分析, 和科学计算, 致力于简化包的管理和部署。Anaconda使用软件包管理系统Conda进行包管理。

在 https://www.anaconda.com/download/#macos 网址中下载Anaconda。

#### Conda是什么？

conda 是开源包（packages）和虚拟环境（environment）的管理系统。

- packages 管理： 可以使用 conda 来安装、更新 、卸载工具包 ，并且它更关注于数据科学相关的工具包。在安装 anaconda 时就预先集成了像 Numpy、Scipy、 pandas、Scikit-learn 这些在数据分析中常用的包。另外值得一提的是，conda 并不仅仅管理Python的工具包，它也能安装非python的包。比如在新版的 Anaconda 中就可以安装R语言的集成开发环境 Rstudio。
- 虚拟环境管理： 在conda中可以建立多个虚拟环境，用于隔离不同项目所需的不同版本的工具包，以防止版本上的冲突。对纠结于 Python 版本的同学们，我们也可以建立 Python2 和 Python3 两个环境，来分别运行不同版本的 Python 代码。

#### Anaconda的优点

Anaconda通过管理工具包、开发环境、Python版本，大大简化了你的工作流程。不仅可以方便地安装、更新、卸载工具包，而且安装时能自动安装相应的依赖包，同时还能使用不同的虚拟环境隔离不同要求的项目。

#### Anaconda内置多项应用

- Anaconda Navigator：用于管理工具包和环境的图形用户界面，众多管理命令也可以在 Navigator 中手工实现
- Jupyter notebook ：基于web的交互式计算环境，可以编辑易于人们阅读的文档，用于展示数据分析的过程
- qtconsole ：一个可执行 IPython 的仿终端图形界面程序，相比 Python Shell 界面，qtconsole 可以直接显示代码生成的图形，实现多行代码输入执行，以及内置许多有用的功能和函数
- spyder ：一个使用Python语言、跨平台的、科学运算集成开发环境

### 安装Anaconda

打开Anaconda安装包安装，一直点继续，直到安装完成。





![img](https://upload-images.jianshu.io/upload_images/6984486-d8c3844271544c16.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image

### 使用Anaconda Navigator

Anaconda安装后，可以从菜单中看到它包含几个应用程序，其中Anaconda Navigator是这几个程序的导航入口。
Anaconda Navigator是Anaconda发行包中包含的桌面图形界面，可以用来方便地启动应用、方便的管理conda包、环境和频道，不需要使用命令行的命令。Navigator可以从Anaconda Cloud或本地Anaconda仓库中搜索包。提供了Windwos、maxOS和Linux版本。Anaconda Navigator主界面如下：





![img](https://upload-images.jianshu.io/upload_images/6984486-57a192ff9d667552.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image

在左边菜单栏中可以看到四个选项，一般常用的是Home和Environments。Environments是你搭建开发环境的地方，你可以在Environments中创建一个开发环境，然后下载所需要的包即可。例如：

##### 创建开发环境

点击左下角create，弹出创建开发环境框，输入环境名和选择python类型即可。



![img](https://upload-images.jianshu.io/upload_images/6984486-59d3373db6198a61.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/644/format/webp)

image





![img](https://upload-images.jianshu.io/upload_images/6984486-3721cb33ff640325.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1156/format/webp)

image

##### 下载tensorflow包

搜索tensorflow包，勾选要下载的包，然后点击右下角Apply即可。





![img](https://upload-images.jianshu.io/upload_images/6984486-50b085bf9d50ffba.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image

Home是你搭建完开发环境后的工作台，在这里可以点击notebook来编写程序。例如：

##### 选择开发环境

在Home工作台中，选择你要使用的工作台。





![img](https://upload-images.jianshu.io/upload_images/6984486-d13a135f5cf47914.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image

在工作台中你可以看到多种应用。例如：

- Jupyter Notebook
- Orange App
- QTConsole
- Glueviz
- Spyder
- RStudio

如果应用没有安装，可以点击应用的Install即可安装。如果已安装，点击Launch即可运行。





![img](https://upload-images.jianshu.io/upload_images/6984486-7919ab7ad51efc54.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image

在这里我们点击运行Jupyter notebook来编写我们的tensorflow程序。

#### Jupyter notebook是什么？

Jupyter notebook 是一种 Web 文档。写过项目的都知道，我们在编译器写代码，然后又去打开word或者其他的文本编辑工具去写开发文档，而且调试也不是非常的方便，是不是感觉特麻烦。 Jupyte的出现就解决我们的各种麻烦，能够让我们把文本，图像和代码全部组合在一个文档中，而且，调试也特别的方便，大大的提高我们开发的效率。

以上内容是我们需要搭建Anaconda开发环境的全部内容。搭建完成后，你就可以编写tensorflow的相关程序啦。