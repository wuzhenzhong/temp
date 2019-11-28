# Redux FAQ：存储设置（Store Setup）

- 贡献者2人

  

## 目录

- 我可以或应该创建多个存储吗？我可以直接导入我的存储，并在组件中使用它吗？

- 我的存储增强器中有多个中间件链可以吗？下一步和中间件功能的调度有什么区别？

- 我如何只订阅状态的一部分？我是否可以将订单中的分派操作作为订阅的一部分？

## 存储设置

### 可以或应该创建多个存储？我可以直接导入我的存储，并在组件中使用它吗？

原始 Flux 模式描述了在应用程序中拥有多个“存储”，每个应用程序拥有不同的域数据区域。这会导致需要有一个商店“ `waitFor`”更新另一个存储等问题。在 Redux 中这不是必需的，因为数据域之间的分离已经通过将单个还原器分解为更小的还原器来实现。

与其他几个问题一样，*可以*在页面中创建多个不同的 Redux 存储，但预期的模式是只有一个存储。拥有一个存储可以使用 Redux DevTools，使数据持久化和再水化更简单，并简化订购逻辑。

在 Redux 中使用多个存储的一些有效原因可能包括：

- 通过分析应用程序进行确认，解决由于状态某些部分的更新太频繁而导致的性能问题。

- 将 Redux 应用程序隔离为更大应用程序中的组件，在这种情况下，您可能需要为每个根组件实例创建一个存储。

但是，创建新存储不应该是您的第一本能，特别是如果您来自 Flux 背景。如果不能解决您的问题，请先尝试减速器组合物，并仅使用多个商店。

同样，虽然您*可以*通过直接导入存储实例*来*引用存储实例，但这不是 Redux 中的推荐模式。如果您创建一个存储实例并从一个模块中导出它，它将成为一个单例。这意味着将 Redux 应用程序隔离为更大应用程序的组件（如果有必要）或启用服务器呈现将变得更加困难，因为在服务器上，您希望为每个请求创建单独的存储实例。

使用 [React Redux 时](https://github.com/reactjs/react-redux)，`connect()`函数生成的包装类实际上会查找`props.store`它是否存在，但如果您将根组件包装进来，`<Provider` `store={store}>`并且让 React Redux 担心将存储传递给它，那么这是最好的。通过这种方式，组件不需要担心导入存储模块，隔离 Redux 应用程序或启用服务器渲染稍后更容易。

#### 更多信息

**文档**

- API: Store**Discussions**

- [#1346: Is it bad practice to just have a 'stores' directory?](https://github.com/reactjs/redux/issues/1436)

- [Stack Overflow: Redux multiple stores, why not?](http://stackoverflow.com/questions/33619775/redux-multiple-stores-why-not)

- [Stack Overflow: Accessing Redux state in an action creator](http://stackoverflow.com/questions/35667249/accessing-redux-state-in-an-action-creator)

- [Gist: Breaking out of Redux paradigm to isolate apps](https://gist.github.com/gaearon/eeee2f619620ab7b55673a4ee2bf8400)

### 我的存储增强器中有多个中间件链可以吗？是什么区别`next`，并`dispatch`在中间件的功能？

Redux 中间件像链接列表一样工作。每个中间件功能都可以调用`next(action)`将行为传递给下一个中间件，调用`dispatch(action)`重新开始列表中的处理，或者不做任何事情来阻止进一步处理该行为。

这个中间件链由传递给`applyMiddleware`创建存储时使用的函数的参数定义。定义多个链将无法正常工作，因为它们将具有明显不同的`dispatch`引用，并且不同的链将有效地断开连接。

#### 更多信息

**文档**

- Advanced: Middleware

- API: applyMiddleware

**讨论**

- [#1051: Shortcomings of the current applyMiddleware and composing createStore](https://github.com/reactjs/redux/issues/1051)

- [Understanding Redux Middleware](https://medium.com/@meagle/understanding-87566abcfb7a)

- [Exploring Redux Middleware](http://blog.krawaller.se/posts/exploring-redux-middleware/)

### 我如何只订阅状态的一部分？我是否可以将订单中的分派操作作为订阅的一部分？

Redux提供了`store.subscribe`一种通知侦听器该商店已更新的单一方法。监听器回调没有收到当前状态作为参数 - 这只是表明*某些事情*已经改变。然后用户逻辑可以调用`getState()`以获取当前状态值。

此API旨在用作低级别原语，不存在依赖性或复杂性，可用于构建更高级别的订阅逻辑。UI绑定（如React Redux）可以为每个连接的组件创建订阅。也可以编写能够智能地比较旧状态和新状态的函数，并且在某些部分发生变化时执行其他逻辑。例子包括[终极版手表](https://github.com/jprichardson/redux-watch)，[终极版订阅](https://github.com/ashaffer/redux-subscribe)和[终极版订户](https://github.com/ivantsov/redux-subscriber)，其提供了不同的方法来指定订阅和处理的变化。

新状态不会传递给监听器，以便简化实施商店增强器（如Redux DevTools）。另外，用户意图对状态值本身做出反应，而不是行为。如果操作很重要并需要专门处理，则可以使用中间件。

#### 更多信息

**文档**

- Basics: Store

- API: Store

**讨论**

- [#303: subscribe API with state as an argument](https://github.com/reactjs/redux/issues/303)

- [#580: Is it possible to get action and state in store.subscribe?](https://github.com/reactjs/redux/issues/580)

- [#922: Proposal: add subscribe to middleware API](https://github.com/reactjs/redux/issues/922)

- [#1057: subscribe listener can get action param?](https://github.com/reactjs/redux/issues/1057)

- [#1300: Redux is great but major feature is missing](https://github.com/reactjs/redux/issues/1300)

**文库**

- [Redux Addons Catalog: Store Change Subscriptions](https://github.com/markerikson/redux-ecosystem-links/blob/master/store.md#store-change-subscriptions)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18