# 动机（Motivation）

- 贡献者1人

  

随着对 JavaScript 单页应用程序的要求变得越来越复杂，**我们的代码必须比以往更多地处理状态**。此状态可以包括服务器响应和缓存数据，以及本地创建的尚未保存到服务器的数据。UI 状态的复杂性也在增加，因为我们需要管理活动路线，选定标签，旋钮，分页控件等。

管理这个不断变化的状态是困难的。如果模型可以更新另一个模型，则视图可以更新模型，该模型会更新另一个模型，而这又会导致另一个视图更新。在某种程度上，您不再理解您的应用程序会发生什么情况，因为您已经**失去了对其状态的何时，为何和如何的控制。**当系统不透明且不确定时，很难重现错误或添加新功能。

好像这还不够糟糕，请考虑**新的要求在前端产品开发中变得常见**。作为开发人员，我们需要处理乐观的更新，服务器端渲染，在执行路由转换之前获取数据等等。我们发现自己试图去处理一个我们以前从来没有处理过的复杂问题，而且我们不可避免地提出这个问题：[是放弃的时候了吗？](http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html)答案是**否定的。**

这种复杂性很难处理，因为**我们正在混合两个**对人类头脑非常难以推理的**概念**：**突变和异步性。**我把它们叫做[曼托斯和可乐](https://en.wikipedia.org/wiki/Diet_Coke_and_Mentos_eruption)。两者在分离方面都很出色，但它们一起造成一团糟。像 [React ](http://facebook.github.io/react)这样的库试图通过去除异步和直接 DOM 操作来解决视图层中的这个问题。但是，管理数据的状态由您决定。这是 Redux 进入的地方。

通过 [Flux](http://facebook.github.io/flux)，[CQRS ](http://martinfowler.com/bliki/CQRS.html)和 [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html)的 步骤，**Redux 尝试**通过对更新发生的方式和时间施加某些限制**来使状态变化可预测**。这些限制反映在 Redux 的三个原则中。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18