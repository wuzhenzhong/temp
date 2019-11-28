# Redux FAQ: 减速器（Reducers）

- 贡献者2人

  

## 目录

- 我如何在两个减速器之间共享状态？我必须使用联合减速器吗？

- 我是否必须使用 switch 语句来处理操作？

## 减速器

### 我如何在两个减速器之间共享状态？我必须使用`combineReducers`吗？

Redux 存储的建议结构是通过密钥将状态对象分割为多个“切片”或“域”，并提供单独的缩减器功能来管理每个单独的数据切片。这与标准Flux模式具有多个独立存储的方式类似，并且 Redux 提供`combineReducers`实用功能以使该模式更容易。但是，需要注意的`combineReducers`是，这*并不是*必需的 - 对于每个状态切片具有单个缩减器函数的常见用例而言，它只是一个实用程序函数，其中包含数据的普通JavaScript对象。

许多用户后来想尝试在两个缩减器之间共享数据，但发现`combineReducers`不允许他们这样做。有几种方法可以使用：

- 如果减速器需要知道来自另一个状态片的数据，则可能需要重新组织状态树形状，以便单个还原器处理更多的数据。

- 您可能需要编写一些自定义函数来处理其中的一些操作。这可能需要用`combineReducers`您自己的顶级减速器功能进行更换。您还可以使用诸如[reduce-reducers之](https://github.com/acdlite/reduce-reducers)类的实用程序来运行`combineReducers`以处理大多数操作，但也可以针对跨状态切片的特定操作运行更专用的[缩减](https://github.com/acdlite/reduce-reducers)器。

- 异步动作创建者（如[redux-thunk）](https://github.com/gaearon/redux-thunk)可以访问整个状态`getState()`。动作创建者可以从状态中检索附加数据并将其放入动作中，以便每个缩减器都有足够的信息来更新其自己的状态片。

一般来说，请记住 reducer 只是函数 - 您可以按任何您想要的方式对它们进行组织和细分，并鼓励您将它们分解为更小，可重用的函数（ “reducer composition” ）。尽管如此，如果子Reducer需要附加数据来计算其下一个状态，则可以从父 Reducer 传递一个自定义的第三个参数。你只需要确保它们一起遵循 reducer 的基本规则：`(state, action) => newState`并且不断更新状态而不是直接改变状态。

#### 更多信息

**文档**

- API: combineReducers

- Recipes: Structuring Reducers

**讨论**

- [#601: A concern on combineReducers, when an action is related to multiple reducers](https://github.com/reactjs/redux/issues/601)

- [#1400: Is passing top-level state object to branch reducer an anti-pattern?](https://github.com/reactjs/redux/issues/1400)

- [Stack Overflow: Accessing other parts of the state when using combined reducers?](http://stackoverflow.com/questions/34333979/accessing-other-parts-of-the-state-when-using-combined-reducers)

- [Stack Overflow: Reducing an entire subtree with redux combineReducers](http://stackoverflow.com/questions/34427851/reducing-an-entire-subtree-with-redux-combinereducers)

- [Sharing State Between Redux Reducers](https://invalidpatent.wordpress.com/2016/02/18/sharing-state-between-redux-reducers/)

### 我是否必须使用该`switch`陈述来处理行为？

不可以。您可以使用任何您想要对减速器中的操作做出响应的方法。该`switch`语句是最常用的方法，但可以使用`if`语句，函数查找表或者创建一个将其抽象化的函数。事实上，虽然Redux 确实需要动作对象包含一个`type`字段，但您的还原器逻辑甚至不必依赖该动作来处理该动作。也就是说，标准方法肯定是使用 switch 语句或基于查找表`type`。

#### 更多信息

**文档**

- Recipes: Reducing Boilerplate

- Recipes: Structuring Reducers - Splitting Reducer Logic

**讨论**

- [#883: take away the huge switch block](https://github.com/reactjs/redux/issues/883)

- [#1167: Reducer without switch](https://github.com/reactjs/redux/issues/1167)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18