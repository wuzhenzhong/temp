# 响应式工具 | Responsive utilities

## Responsive utilities

为了更快速地进行适合移动设备的开发，请使用这些实用程序类通过媒体查询显示和隐藏设备的内容。还包括打印时切换内容的实用程序类。

尝试在有限的基础上使用这些内容，并避免创建同一网站的完全不同版本。相反，使用它们来补充每个设备的显示。

### 可用的类

使用单个或组合的可用类切换视口断点上的内容。

|               | Extra small devices Phones (<768px) | Small devices Tablets (≥768px) | Medium devices Desktops (≥992px) | Large devices Desktops (≥1200px) |
| :------------ | :---------------------------------- | :----------------------------- | :------------------------------- | :------------------------------- |
| .visible-xs-* | Visible                             | Hidden                         | Hidden                           | Hidden                           |
| .visible-sm-* | Hidden                              | Visible                        | Hidden                           | Hidden                           |
| .visible-md-* | Hidden                              | Hidden                         | Visible                          | Hidden                           |
| .visible-lg-* | Hidden                              | Hidden                         | Hidden                           | Visible                          |
| .hidden-xs    | Hidden                              | Visible                        | Visible                          | Visible                          |
| .hidden-sm    | Visible                             | Hidden                         | Visible                          | Visible                          |
| .hidden-md    | Visible                             | Visible                        | Hidden                           | Visible                          |
| .hidden-lg    | Visible                             | Visible                        | Visible                          | Hidden                           |

从v3.2.0开始，`.visible-*-*`每个断点的类都有三种变化，每种类型`display`下面列出了一个CSS 属性值。

| Group of classes        | CSS display            |
| :---------------------- | :--------------------- |
| .visible-*-block        | display: block;        |
| .visible-*-inline       | display: inline;       |
| .visible-*-inline-block | display: inline-block; |

因此，对于超小型（`xs`）屏幕，可用`.visible-*-*`类：`.visible-xs-block`，`.visible-xs-inline`，和`.visible-xs-inline-block`。

这些类`.visible-xs`，`.visible-sm`，`.visible-md`，并且`.visible-lg`也存在，但**不赞成V3.2.0的**。`.visible-*-block`除了额外的切换`<table>`元素的特殊情况外，它们大致相当。

### 打印类

与常规响应类类似，请使用这些类别来切换要打印的内容。

| Classes                                                      | Browser | Print   |
| :----------------------------------------------------------- | :------ | :------ |
| .visible-print-block .visible-print-inline .visible-print-inline-block | Hidden  | Visible |
| .hidden-print                                                | Visible | Hidden  |

该类`.visible-print`也存在，但从v3.2.0开始**已**被**弃用**。这大致相当于`.visible-print-block`，除了`<table>`相关元素的附加特殊情况。

### 测试用例

调整浏览器大小或在不同设备上加载以测试响应式实用程序类。

#### 可见...

绿色的复选标记表示该元素在当前视口中**可见**。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#responsive-utilities-tests)

#### 隐藏在......

这里，绿色的选中标记也表示该元素**隐藏**在当前的视口中。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#responsive-utilities-tests)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-10-08