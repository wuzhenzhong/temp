# 数据流（Data Flow）

- 贡献者1人

  

Redux 体系结构围绕**严格的单向数据流进行**。

这意味着应用程序中的所有数据都遵循相同的生命周期模式，使您的应用程序的逻辑更具可预测性并更易于理解。它还鼓励数据规范化，以便最终不会出现多个相互不知道的相同数据的独立副本。

如果您仍然不确定，请阅读 Motivation 和 [The Case for Flux](https://medium.com/@dan_abramov/the-case-for-flux-379b7d1982c6)，以获得支持单向数据流的引人注目的论点。虽然 Redux 不完全是 Flux，但它具有相同的关键优势。

任何 Redux 应用程序中的数据生命周期都遵循以下4个步骤：

1. **你调用** `store.dispatch(action)`。动作是一个描述*发生的事情*的简单对象。例如： { type: 'LIKE_ARTICLE', articleId: 42 } { type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } } { type: 'ADD_TODO', text: 'Read the Redux docs.' } 将行动看作是一个非常简短的新闻片段。“玛丽喜欢第42条”或“阅读Redux文档”。被添加到todos列表中。“您可以`store.dispatch(action)`从应用程序中的任何位置调用，包括组件和XHR回调，甚至可以按照预定的时间间隔进行调用。
2. **Redux 商店调用您提供的 reducer 功能。** 商店将向 reducer 传递两个参数：当前状态树和操作。例如，在 todo 应用程序中，根缩减器可能会收到如下所示的内容： // The current application state (list of todos and chosen filter) let previousState = { visibleTodoFilter: 'SHOW_ALL', todos: { text: 'Read the docs.', complete: false } } // The action being performed (adding a todo) let action = { type: 'ADD_TODO', text: 'Understand the flow.' } // Your reducer returns the next application state let nextState = todoApp(previousState, action) 注意 reducer 是一个纯函数。它只*计算*下一个状态。它应该是完全可预测的：多次调用相同的输入应该产生相同的输出。它不应该执行API调用或路由器转换等任何副作用。这些应该在动作发出之前发生。
3. **根减速器可以将多个减速器的输出组合成单个状态树。**您如何构建根部缩减器完全取决于您。Redux 附带一个`combineReducers()`辅助函数，用于将根简化器“分解”为单独的函数，每个函数都管理状态树的一个分支。 这是如何`combineReducers()`工作。假设你有两个reducer，一个用于todos列表，另一个用于当前选择的过滤器设置： function todos(state = [], action) { // Somehow calculate it... return nextState } function visibleTodoFilter(state = 'SHOW_ALL', action) { // Somehow calculate it... return nextState } let todoApp = combineReducers({ todos, visibleTodoFilter }) 然后它会将两组结果合并为一个状态树： return {todos：nextTodos，visibleTodoFilter：nextVisibleTodoFilter } 虽然`combineReducers()`是一个方便的助手实用程序，您不必使用它；随时写你自己的根减速器！
4. **Redux 存储保存由根减速器返回的完整状态树。** 这棵新树现在是你的应用程序的下一个状态！现在每个注册的`store.subscribe(listener)`监听者都会被调用；听众可能会调用`store.getState()`来获取当前状态。现在，UI 可以更新以反映新的状态。如果你使用 [React Redux ](https://github.com/gaearon/react-redux)这样的绑定，这就是`component.setState(newState)`被调用的地方。

## 下一步

现在您已经知道 Redux 的工作原理了，让我们将其连接到 React 应用程序。

> 高级用户注意事项
>
> 如果您已经熟悉基本概念并已完成本教程，请不要忘记查看高级教程中的异步流程，以了解中间件在异步操作到达还原器之前如何转换它们。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18