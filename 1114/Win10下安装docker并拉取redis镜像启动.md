# Win10下安装docker并拉取redis镜像启动

闲来无事学习新知识,准备学习一下当下比较热的docker,本篇主要介绍在win10系统下安装docker并拉取redis镜像进行启动,win10系统需要是专业版的,如果是家庭版则需要下载docker-toolbox.[toolbox下载地址](http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/)

![图片描述](https://user-gold-cdn.xitu.io/2019/11/14/16e688f16bc8cfbd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

选择下面的下载,上面的是需要收费版的. 当然还有更简单粗暴的方法,**直接把win10家庭版升级成专业版的,某宝上十几块钱一个激活码.**

### 1.安装docker

去官网先下载一个docker,[官网地址](https://docs.docker-cn.com/). 记得要确保开启Hyper-V这个组件才能安装Docker，注意如果BIOS中没有开启虚拟功能也不行，一般默认是开启的。（注意Docker和VMWare虚拟机是不能同时使用的，所以如要使用VMWare就要先关闭Hyper-V功能）.

![图片描述](https://user-gold-cdn.xitu.io/2019/11/14/16e688f1613c68d0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

然后安装刚才下载的安装包,一直下一步就可以.然后你就回喜提一个小鲸鱼. 如果你之前安装过toolbox一定要卸载后再安装docker,否则会报错. 安装好后打开powershell,执行命令`docker version`

![图片描述](https://user-gold-cdn.xitu.io/2019/11/14/16e688f16879703f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如果这时候报错有可能是之前安装了toolbox,toolbox安装后会在环境变量中进行配置,只需要将环境变量中的docker系列的删除就行.



### 2 拉取redis

打开PowerShell，输入`docker pull redis`命令来下载redis镜像，默认下载最新版本的redis镜像。

![图片描述](https://user-gold-cdn.xitu.io/2019/11/14/16e688f225e594de?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

等待下载完成即可, 接着启动redis,输入命令



```
docker run -d -p 6379:6379 --name redis01 redis
复制代码
```

如果想停止使用命令

```
docker stop [Name]
复制代码
```

删除容器使用:

```
docker rm [Name]
```