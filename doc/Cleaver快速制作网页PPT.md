# Cleaver快速制作网页PPT

2015.10.16 17:10:11字数 793阅读 925

### **写在开始之前**

互联网时代，以浏览器作为入口，已经有越来越多的桌面应用被web应用所取代。微软最赚钱的Office办公软件，也正在被免费的web应用所吞噬。如今即便薄学如我就已然知晓`reveal.js` `impress.js`等各种轮子来制作体验优良的网页PPT。今天就来尝试下这Node.js的轮子`Cleaver`来制作网页PPT。

Cleaver基于HTML5，用最短的时间做出超炫幻灯片。你还会坚持PowerPoint吗？

### **Cleaver介绍**

如果你已经有了一个Markdown的文档，30秒就可以制作成幻灯片。Cleaver是为Hacker准备的工具。
Cleaver的官方发布页：[http://jdan.github.io/cleaver/](https://link.jianshu.com/?t=http://jdan.github.io/cleaver/)；
参考文章:[30秒制作幻灯片 Cleaver](https://link.jianshu.com/?t=http://blog.fens.me/nodejs-slide-cleaver/)。

### **Cleaver安装**

系统环境(据悉：cleaver不支持win系统)
Linux: Ubuntu 12.04 LTS 64bit
node: v0.6.12
npm: 1.1.4
通过nodejs安装Cleaver

```shell
//局部安装Cleaver
~ mkdir nodejs-cleaver && cd nodejs-cleaver
~ sudo npm install cleaver

//全局安装Cleaver
~ sudo npm install cleaver -g
```

### **Cleaver基本使用**

执行cleaver命令，解析一个markdown文件，会自动生成HTMl文档，欧耶,算是Pandoc的部分功能深入了;

用cleaver自带的例子：

```shell
//如是局部安装，运行下面一句就好
cleaver node_modules/cleaver/examples/basic.md

//如是全局安装，请跑起Like this：
cleaver  /usr/local/lib/node_modules/cleaver/examples/basic.md
```

### **Cleaver配置选项option**

```shell
title: Basic Example  #HTML的标题名
author:               #作者信息,在最后一页显示
  name: Jordan Scales
  twitter: jdan
  url: http://jeffjade.com
output: basic.html
```

除此之外还有其他额外设置，譬如：

- theme: 皮肤(theme: jdan/cleaver-retro)
- style: css样式表(style: css/main.css)
- output: 生成的HTML文件名(output: basic.html)
- controls: 控制按钮(controls: true)
- progress: 顶部显示进程条(progress: true)
- agenda: 生成一个目录页(progress: false)
- encoding: 文档的字符编码(encoding: utf-8)
- template: 设置每张slide的模板(template: template/slide.mustache)
- layout: 设置HTML模板(template: template/layout.mustache)

### **Cleaver的正文内容:**

Cleaver仅支持Markdown语法的，通过Markdown构建网页内容，Markdown官方网站：[http://daringfireball.net/projects/markdown/](https://link.jianshu.com/?t=http://daringfireball.net/projects/markdown/)

需要注意的是，Cleaver以” — “(两个中横线)做为分页的标志，其他语法都遵循Markdown规则。

MS Office PowerPoint 正在被渐行渐远。。。

### **写在最后**

相比于之下，`impress.js`, `reveal.js`以及Cleaver三者各有特色吧；在唯快不破的理论下，Cleaver自然占据了无与伦比的优势，只是像动画，样式等比于impress是一个短板。并且，在写此文的时候欲以Cleaver来呈现的，不过总是报出下面这样的问题(Mac OS X Yosemite)：

> !! Cannot read property 'compiled' of undefined
> Please report this to [https://github.com/chjj/marked](https://link.jianshu.com/?t=https://github.com/chjj/marked).

花了些许时间来探究这个问题，只是现在网络上目测关于这个信息不是很多。逐一对比之下，文章的格式没什么问题。在即将放弃的时候，发现原来是标记代码的时候用了````shell`,所造成的问题,去掉shell或者换成别的标记就没有问题了。额...。

这里是[此文的PPT版本(Cleaver)](https://link.jianshu.com/?t=http://www.jeffjade.com/special/md2pptViaCleaver.html);当然，这个是需要在PC偏高些的浏览器上才能正常查看。

原文链接:[http://www.jeffjade.com/2015/10/15/2015-10-16-cleaver-make-ppt/](https://link.jianshu.com/?t=http://www.jeffjade.com/2015/10/15/2015-10-16-cleaver-make-ppt/)