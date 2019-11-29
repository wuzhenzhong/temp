# 概览 | Overview

- 贡献者1人

  

## 概观

了解Bootstrap基础架构关键部分的原理，包括更好，更快，更强大的Web开发方法。

### HTML5文档类型

引导程序使用某些HTML元素和CSS属性，这些属性需要使用HTML5文档类型。将它包含在所有项目的开始处。

```javascript
<!DOCTYPE html>
<html lang="en">
  ...
</html>
```

### 移动端优先

借助Bootstrap 2，我们为框架的关键部分添加了可选的移动友好样式。借助Bootstrap 3，我们将该项目从一开始就改写为移动友好。他们没有添加可选的移动样式，而是直接进入了核心。事实上，**Bootstrap首先是移动的**。移动第一种样式可以在整个库中找到，而不是在单独的文件中找到。

为了确保正确的渲染和触摸缩放，将 viewport 元标签添加到您的`<head>`。

```javascript
<meta name="viewport" content="width=device-width, initial-scale=1">
```

您可以通过添加`user-scalable=no`到视口元标记来禁用移动设备上的缩放功能。这将禁用缩放功能，这意味着用户只能滚动，并且您的网站的结果感觉更像是本机应用程序。总的来说，我们不建议在每个网站上都这样做，所以请谨慎使用！

```javascript
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

### 排版和链接

Bootstrap设置基本的全局显示，排版和链接样式。具体而言，我们：

- 设置`background-color: #fff;`在`body`上

- 使用`@font-family-base`，`@font-size-base`和`@line-height-base`属性作为我们的基础

- 通过设置全局链接颜色`@link-color`并仅应用链接下划线在`:hover`

这些样式可以在里面找到`scaffolding.less`。

### Normalize.css

为了改进跨浏览器渲染，我们使用了[Nicolas Gallagher](https://twitter.com/necolas)和[Jonathan Neal的](https://twitter.com/jon_neal)[Normalize.css](http://necolas.github.io/normalize.css/)。

### 容器

Bootstrap需要一个包含元素来包装网站内容并容纳我们的网格系统。您可以选择两个容器中的一个用于您的项目。请注意，由于`padding`和更多，两个容器都不能嵌套。

使用`.container`一个响应固定宽度的容器。

```javascript
<div class="container">
  ...
</div>
```

使用`.container-fluid`了全宽的容器，跨越视口的整个宽度。

```javascript
<div class="container-fluid">
  ...
</div>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-10-08