# 术语词汇表（Glossary）

- 贡献者1人

  

这是 Redux 中核心术语的词汇表，以及它们的类型签名。这些类型使用 [Flow符号 ](http://flowtype.org/docs/quick-reference.html)进行记录。



[纠错](javascript:;)

## State

```javascript
type State = any
```

*State*（也称为 state tree）是一个广义术语，但在 Redux API 中它通常指的是由存储管理并返回的单一状态值`getState()`。它表示 Redux 应用程序的整个状态，通常是深度嵌套的对象。

按照惯例，顶层状态是一个对象或像 Map 这样的其他键值集合，但从技术上讲它可以是任何类型。不过，你应该尽最大努力保持状态的序列化。不要把任何东西放在里面，你不能轻易变成JSON 。

## Action

```javascript
type Action = Object
```

一个 *Action* 是一个普通的对象，它表示的意图来改变状态。操作是将数据导入商店的唯一方法。任何数据，无论是来自 UI 事件，网络回调，还是其他来源，如 WebSockets ，都需要最终作为动作分派。

操作必须有一个`type`指示正在执行的操作类型的字段。类型可以定义为常量并从另一个模块导入。因为字符串是可序列化的，所以最好给`type`使用字符串，而不是 Symbols。

除`type`之外，动作对象的结构真的取决于你。如果您有兴趣，请查看 [Flux Standard Action](https://github.com/acdlite/flux-standard-action)，了解如何构建 actions 的建议。

另请参阅下面的异步操作。

## Reducer

```javascript
type Reducer<S, A> = (state: S, action: A) => S
```

reducer（也称为 reducing 函数）是接受的累积和值，并返回一个新的累积的函数。它们被用来将一组值减少到一个值。

Reduux 并不是 Redux 独有的 - 它们是函数式编程的基本概念。即使大多数非函数式语言（如 JavaScript ）也有一个用于减少的内置API。在 JavaScript 中，它是 [`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)。

在 Redux 中，累计值是状态对象，正在累积的值是动作。考虑到之前的状态和行动，Reducers 计算出一个新的状态。它们必须是*纯函数* - 对于给定的输入返回完全相同的输出的函数。他们也应该没有副作用。这是什么使激动人心的功能，就像热重载和时间旅行。

Reduux 是 Redux 中最重要的概念。

*不要将 API 调用放入 reducer 中。*

## 调度函数

```javascript
type BaseDispatch = (a: Action) => Action
type Dispatch = (a: Action | AsyncAction) => any
```

*调度函数*是接受动作或异步动作的函数，那么它可能会或可能不会向商店分配一个或多个动作。

我们必须区分一般的调度功能和没有任何中间件的商店实例提供的基本`dispatch`函数。

基本调度功能*始终*同步向商店的 reducer 发送一个动作，以及商店返回的以前的状态，以计算新的状态。它预计行动是简单的对象准备被 reducer 消耗。

中间件包装基本调度函数。它允许调度函数除了操作之外还处理异步操作。中间件可能会在将它们传递到下一个中间件之前转换，延迟，忽略或解释操作或异步操作。请参阅下面的详细信息。

## Action Creator

```javascript
type ActionCreator = (...args: any) => Action | AsyncAction
```

一个 *action 的创建者*是很简单，创建一个动作的函数。不要混淆这两个术语 - 再一次，动作是信息的载荷，动作创建者是创建动作的工厂。

调用动作创建者只会产生一个动作，但不会发送它。您需要调用商店的`dispatch`功能来实际导致突变。有时我们说*绑定动作创建者*意味着调用动作创建者的函数，并立即将其结果分派给特定的商店实例。

如果动作创建者需要读取当前状态，执行 API 调用或导致副作用（如路由转换），则应该返回异步操作而不是操作。

## Async Action

```javascript
type AsyncAction = any
```

一个*异步操作*是发送到调度功能的价值，但还没有准备好由 Reducer 消费。在发送到基本`dispatch()`函数之前，它将被中间件转换为一个动作（或一系列动作）。异步操作可能有不同的类型，具体取决于您使用的中间件。它们通常是异步原语，如 Promise 或 Thunk，它们不会立即传递给 reducer，而是在操作完成后触发动作分派。

## 中间件

```javascript
type MiddlewareAPI = { dispatch: Dispatch, getState: () => State }
type Middleware = (api: MiddlewareAPI) => (next: Dispatch) => Dispatch
```

中间件是一个高阶函数，它组成一个派遣函数来返回一个新的派遣函数。它经常将异步行为转化为行为。

中间件使用函数组合来组合。这对记录操作，执行路由等副作用或将异步API调用转换为一系列同步操作非常有用。

有关详细了解中间件的信息，请参阅applyMiddleware（...中间件）。

## Store

```javascript
type Store = {
  dispatch: Dispatch
  getState: () => State
  subscribe: (listener: () => void) => () => void
  replaceReducer: (reducer: Reducer) => void
}
```

Store 是一个持有应用程序状态树的对象。

在 Redux 应用程序中应该只有一个存储区，因为构图发生在缩减级别上。

- `dispatch(action)` 是上述的基本调度功能。

- `getState()` 返回store的当前状态。

- `subscribe(listener)` 注册一个在状态改变时被调用的函数。

- `replaceReducer(nextReducer)`可以用来实现热重载和代码分割。很可能你不会使用它。

查看完整的 store API 参考了解更多详情。

## Store creator

```javascript
type StoreCreator = (reducer: Reducer, preloadedState: ?State) => Store
```

Store creator 是创建 Redux store 的函数。就像调度函数一样，我们必须区分`createStore(reducer, preloadedState)`从 Redux 包导出的基本 store creator 与从 store enhancers返回的 store creator。

## Store enhancer

```javascript
type StoreEnhancer = (next: StoreCreator) => StoreCreator
```

Store enhancer 是一个高级功能，它组成 store creator 返回一个新的增强型 store creator 。这与中间件类似，因为它允许您以可组合的方式更改 store 界面。

Store enhancer 与 React 中的高阶组件有很多相同的概念，它们有时也被称为“组件增强器”。

由于 store 不是实例，而是纯粹的对象函数集合，所以可以轻松创建和修改副本，而不会改变原始 store。有`compose`文件证明这个例子。

很可能你永远不会写 store enhancer ，但你可以使用[开发者工具](https://github.com/gaearon/redux-devtools)提供的。这是让时间旅行可能没有应用程序意识到它正在发生的事情。有趣的是，Redux 中间件实现本身就是一个存储增强器。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18