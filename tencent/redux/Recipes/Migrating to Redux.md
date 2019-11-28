# 迁移至Redux( Migrating to Redux )

- 贡献者1人

  

Redux 不是一个单一的框架，而是一组合同和一些使它们一起工作的功能。大多数“ Redux 代码”甚至不会使用 Redux API，因为大部分时间您都会编写函数。

这使得从 Redux 移来和移去都变得很容易。

我们不想锁定！

## 来自Flux

Reducers 捕捉 Flux Stores 的“本质”，因此无论您使用 [Flummox](http://github.com/acdlite/flummox)，[Alt](http://github.com/goatslacker/alt)，[传统 Flux ](https://github.com/facebook/flux)还是任何其他Flux库，都可以逐步将现有的 Flux 项目迁移到 Redux 。

也可以执行相反的操作，并按照相同的步骤从 Redux 迁移到这些库中的任何一个。

你的过程将如下所示：

- 创建一个名为的函数`createFluxStore(reducer)`，从 reducer 函数创建与现有应用程序兼容的 Flux 存储库。从内部看，它可能与 Redux 的`createStore`（[源](https://github.com/reactjs/redux/blob/master/src/createStore.js)）实现类似。它的调度处理程序应该调用`reducer`任何操作，存储下一个状态并发出变化。

- 这使您可以逐渐将应用程序中的每个Flux Store都重写为Reducer，但仍可导出，`createFluxStore(reducer)`以便其他应用程序不知道发生了这种情况并查看 Flux 商店。

- 在您重写Stores时，您会发现需要避免某些Flux反模式，例如在Stores中获取API或触发商店内的操作。一旦您将代码移植到reducer之后，您的Flux代码将更容易遵循！

- 当您将所有 Flux Stores 移植到 Reduce 之上时，您可以用一个 Redux 存储替换 Flux 库，并使用`combineReducers(reducers)`将已有的 Reduce 库合并为一个。

- 现在剩下要做的就是移动界面以使用 react-redux 或同等功能。

- 最后，您可能想要开始使用一些 Redux 成语，比如中间件来进一步简化异步代码。

## **来自 Backbone**

Backbone 的模型层与 Redux 完全不同，所以我们不建议混合它们。如果可能的话，最好从头开始重写应用程序的模型层，而不是将 Backbone 连接到 Redux 。但是，如果重写不可行，则可以使用 [backbone-redux ](https://github.com/redbooth/backbone-redux)逐步迁移，并使 Redux 存储与 Backbone 模型和集合保持同步。

如果您的 Backbone 代码库太大而不能快速重写，或者您不想管理商店和模型之间的交互，请使用 [backbone-redux-migrator ](https://github.com/naugtur/backbone-redux-migrator)来帮助您的两个代码库共存，同时保持健康的分离。重写完成后，可以丢弃 Backbone 代码，并且一旦配置了路由器，Redux 应用程序就可以自行工作。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18