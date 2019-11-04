# 用windows下的Anaconda搭建Django虚拟环境

[wx2007300875](https://www.jianshu.com/u/80644c56d548)关注

0.1432018.02.07 23:13:59字数 1,328阅读 10,148

Django 是一个Python定制框架，可用于简便、快速的开发数据库驱动的web站点。

要使用Django，首先要建立一个**虚拟工作环境**。

那么，为什么要搭建**虚拟环境**呢？我们来看以下的开发场景：

假设要进行Python web开发，使用的是Django。手上还有两个老项目A和B需要维护，而新项目C也正在开发中。这里项目A使用的是django1.3，项目B使用的是django1.4，而新项目C使用的是Django1.8。那么问题来了，如何同时在本地进行ABC这三个项目的开发和维护？
正常的模式可能是这样：现在在A项目上有一个BUG需要修复，于是，先执行下面的命令，删除掉原来的版本：

```undefined
pip uninstall django
```

然后再执行下面的命令安装django1.3:

```undefined
pip install django==1.3
```

数分钟后，bug修复完毕，好，现在进行新项目C的开发了，然后又要重复上面的故事。
好了，这还是最理想的情况。什么？还有不理想的情况？是的，基于django的第三方依赖也是跟Django版本相关的，于是除了install和uninstall Django之外，还要uninstall和install其依赖，Orz，这特么的就尴尬了...

可见，虚拟环境的搭建是必备的步骤。在windows下，有多种搭建虚拟环境的方法。其中，Anaconda拥有强大而方便的包管理与环境管理的功能，可以轻松创建虚拟工作环境。安装好Anaconda后，可以在“开始菜单-所有程序”中，找到Anaconda Promt，这是一个类似于cmd的终端，可以用命令行来进行包和环境的管理。

现在，我们可以开始虚拟环境的搭建了。

**1、创建环境**

输入以下命令：

```cpp
conda env list
```

可以看到此时仅有一个环境，叫做root，这个环境所在的目录就是Anaconda的安装目录（D:\Anaconda3）。接着，输入以下命令：

```undefined
conda create -n my_env
```

我们就成功地创建了一个新的虚拟工作环境。其中，“my_env”是这个环境的名称，在Anaconda的安装目录下面的envs文件夹下，新建了一个my_env文件夹（D:\Anaconda3\envs\my_env），它就是这个环境所在的目录。

**2、环境管理**

完成环境创建后，我们输入：

```cpp
conda env list
```

此时，我们可以看到有两个工作环境，分别是：root，以及我们刚刚创建的my_env环境。root的目录旁有一个星号（*），代表此时虚拟环境还没有被激活。只有激活它，我们才能为这个虚拟环境进行包的安装和管理，为了激活它，我们输入以下命令：

```undefined
activate my_env
```

此时我们可以看到，命令行的左侧多出了一个（my_env），代表我们当前是在该环境下进行命令行的操作。如果我们此时再输入：conda env list，可以看到星号（*）已经移到了刚刚创建的虚拟环境目录的左侧。需要注意的是，如果关闭了Anaconda Promt，再新再打开的话，那么还需要重新进行一次激活操作。

如果想要移除该环境，我们需要输入以下命令：

```csharp
conda remove -n my_env --all
```

**3、安装Django**

在新的环境激活的情况下，我们在命令行中输入：

```cpp
conda list
```

可以看到，此时该环境下没有安装任何的package。此时，我们开始使用Anaconda强大的包管理功能。在命令行中输入：

```undefined
conda install django
```

它就会自动帮助我们在新环境下，安装Django以及一些配套安装的package。

**4、创建项目**

Django安装完成后，我们在命令行中输入：path，以查看当前环境的工作路径，如果一切正常的话，现在已经生成了一些my_env文件夹下的新路径。我们即将用到的django-admin.py就存放在D:\Anaconda3\envs\my_env\Scripts这个路径中。

现在，可以开始项目的创建了。首先，在命令行中用cd命令切换到我们希望存放项目文件夹的位置，例如：

```bash
cd D:\PycharmProjects\new_env
```

再执行以下命令创建项目：

```undefined
django-admin startproject new_project .
```

后面的空格和句点千万不要忘记，否则部署应用程序时将遭遇一些配置问题。

如果django-admin.py运行失败了，那么在D:\Anaconda3\envs\my_env\Scripts路径中找到这个文件，将它的默认打开方式设为D:\Anaconda3\envs\my_env下的python.exe，再重新尝试执行命令。

最终，new_env文件夹下出现了名为new_project的项目文件夹，以及manage.py文件，于是我们完成了虚拟环境下的第一个项目创建工作。