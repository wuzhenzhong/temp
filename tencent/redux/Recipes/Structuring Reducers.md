# 构造Reducers（Structuring Reducers）

- 贡献者1人

  

Redux 的核心是一个非常简单的设计模式：所有的“写入”逻辑都进入一个单一的函数，运行该逻辑的唯一方法是给 Redux 一个描述发生的事情的简单对象。Redux store 调用写入逻辑函数并传入当前状态树和描述对象，写入逻辑函数返回一些新的状态树，Redux 存储区通知任何订户状态树已更改。

Redux 对如何编写逻辑函数应该如何工作提出了一些基本限制。如 Reducer 中所述，它必须具有签名`(previousState, action) => newState`，被称为***Reducer 函数***，并且必须是*纯粹*且可预测的。

除此之外，Redux 并不关心如何在Reducer函数内实际构造逻辑，只要遵循这些基本规则即可。这既是自由的源泉，也是混淆的源泉。但是，在编写 Reducer 时，有许多常用模式被广泛使用，还有许多相关的主题和概念需要注意。随着应用程序的增长，这些模式在管理 Reducer 代码复杂性，处理实际数据以及优化 UI 性能方面发挥着至关重要的作用。

### 编写 Reducer 的先决条件概念

其中一些概念已经在 Redux 文档的其他地方描述过。其他是通用的，并且适用于 Redux 本身之外，并且有许多现有文章详细说明了这些概念。这些概念和技术构成了编写固态 Redux reducer 逻辑的基础。

在转向更高级和 Redux 专用技术之前，必须先**彻底理解**这些先决条件概念。推荐的阅读列表可在以下网址获得：

#### 先决条件概念

同样重要的是要注意，根据特定应用程序中的体系结构决策，这些建议中的一些可能会或可能不会直接适用。例如，使用 Immutable.js Maps 存储数据的应用程序可能会使用Reduced逻辑，其结构至少有点不同于使用纯 Javascript 对象的应用程序。本文档主要假设使用纯 Javascript 对象，但如果使用其他工具，许多原则仍然适用。

### Reducer 的概念和技术

- 基本 Reducer 结构

- 分裂 Reducer 逻辑

- 重构 Reducers 示例

- 运用 `combineReducers`

- `combineReducers`之外

- 规范化状态

- 更新标准化数据

- 重用 Reducer Logic

- 不变的更新模式

- 初始化状态

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18