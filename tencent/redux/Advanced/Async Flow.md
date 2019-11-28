# 异步流程（Async Flow）

- 贡献者2人

  

没有中间件，Redux 存储只支持同步数据流。这是你默认使用的`createStore()`。

您可能会使用`applyMiddleware()`增强`createStore()`。这不是必需的，但它可以让您以便捷的方式表达异步操作。

像 [redux-thunk ](https://github.com/gaearon/redux-thunk)或 [redux-promise ](https://github.com/acdlite/redux-promise)这样的异步中间件包装了存储中的`dispatch()`方法，并允许你分派除动作之外的其他东西，例如函数或 Promise。您使用的任何中间件都可以解释您发送的任何内容，并且可以将操作传递给链中的下一个中间件。例如，Promise 中间件可以拦截 Promise 并且响应于每个 Promise 异步地分派一对开始/结束动作。

当链中的最后一个中间件调度一个动作时，它必须是一个普通的对象。这是同步 Redux 数据流发生的时间。

检查异步示例的完整源代码。

## 下一步

现在您已经看到了一个中间件可以在 Redux 中执行的例子，现在是时候了解它的实际工作原理以及如何创建自己的。进入下一个关于中间件的详细部分。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18