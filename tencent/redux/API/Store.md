# 存储（Store）

- 贡献者1人

  

一个存储保存你的应用程序的整个状态树。

改变其内部状态的唯一方法是在其上发送一个动作。

一家存储不是一个类。它只是一个有几个方法的对象。

要创建它，请将您的根部缩小函数传递给`createStore`。

> Flux 用户注意事项
>
> 如果您来自 Flux，您需要了解一个重要的差异。Redux 没有分派器或支持许多商店。**相反，只有一个存储具有单一的根** **削减功能。**随着您的应用程序的增长，您不必添加商店，而是将根缩减器分解为更小的减法器，并独立运行于状态树的不同部分。你可以使用一个类似于`combineReducers`的帮手来组合它们。这与React应用程序中只有一个根组件的方式类似，但它由许多小组件组成。

### 存储方法

- `getState()`
- `dispatch(action)`
- `subscribe(listener)`
- `replaceReducer(nextReducer)`

## 存储方法

### `getState()`

返回应用程序的当前状态树。

它等于存储减速器返回的最后一个值。

#### 返回

*（任何）*：您的应用程序的当前状态树。

### `dispatch(action)`

分派一个动作。这是触发变化状态的唯一途径。

存储的减少函数将与当前`getState()`结果和给定的`action`同步进行调用。其返回值将被视为下一个状态。它将从现在开始返回`getState()`，变化监听者将立即得到通知。

> Flux 用户注意事项
>
> 如果您尝试从减速器内部调用`dispatch`，它将抛出一个错误，指出“减速器可能不派遣动作”。这与 Flux 中的“无法在派送中间派送”错误类似，不会导致与之相关的问题。在 Flux 中，存储正在处理操作并发布更新时禁止发送。这是不幸的，因为它不可能从组件生命周期挂钩或其他良性地点派发操作。在 Redux 中，订阅在根减速器返回新状态后调用，因此您*可能会这样做*在订阅监听器中分派。您仅被禁止在减速器内发送，因为它们不得有副作用。如果您想针对某个操作产生副作用，则可以在可能的异步操作创建器中执行此操作。

#### 参数

1. `action`（*Object* †）：描述对您的应用程序有意义的更改的普通对象。动作是将数据存入存储区的唯一方式，因此无论是来自 UI 事件，网络回调还是其他来源（如 WebSockets）的任何数据都需要最终作为操作分派。操作必须有一个`type`指示正在执行的操作类型的字段。类型可以定义为常量并从另一个模块导入。因为字符串是可序列化的，所以最好使用`type`比[符号](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol)更多的字符串。除此`type`之外，动作对象的结构真的取决于你。如果您有兴趣，请查看[Flux 标准行动部门](https://github.com/acdlite/flux-standard-action)，了解如何构建行动的建议。

#### 返回

（Object†）：已调度的操作（请参阅注释）。

#### 注释

†通过调用`createStore`获得的“vanilla”存储实现仅支持简单对象操作，并立即将其交给 reducer。

但是，如果用`applyMiddleware`包装`createStore`，中间件可以有不同的解释的行动，并提供调度异步操作的支持。异步操作通常是 Promise，Observables 或 Thunk 等异步基元。

中间件由社区创建，默认情况下不会与 Redux 一起提供。您需要显式安装像 [redux-thunk ](https://github.com/gaearon/redux-thunk)或 [redux-promise ](https://github.com/acdlite/redux-promise)这样的软件包才能使用。您也可以创建自己的中间件。

要了解如何描述异步 API 调用，请阅读操作创建者中的当前状态，执行副作用或链接它们以按顺序执行，请参阅示例`applyMiddleware`。

#### 示例

```javascript
import { createStore } from 'redux'
let store = createStore(todos, ['Use Redux'])

function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

store.dispatch(addTodo('Read the docs'))
store.dispatch(addTodo('Read about the middleware'))
```

### `subscribe(listener)`

添加更改侦听器。任何时候都会调用它，并且状态树的某些部分可能会发生更改。然后您可以调用`getState()`以读取回调中的当前状态树。

您可以通过更改监听器进行`dispatch()`调用，但需注意以下事项：

1. 监听器只应调用`dispatch()`以响应用户操作或特定条件（例如，当商店具有特定字段时调度动作）。没有任何条件的调用`dispatch()`在技术上是可能的，但是它会导致无限循环，因为每次调用`dispatch()`通常会再次触发监听器。
2. 在每次`dispatch()`调用之前都会订阅被快照。如果您在调用侦听器时订阅或取消订阅，这对`dispatch()`当前正在进行的操作没有任何影响。但是，下一次`dispatch()`调用（无论是否嵌套）将使用订阅列表的更新近期快照。
3. 监听器不应该期望看到所有的状态变化，因为在监听器被调用之前，在`dispatch()`嵌套期间状态可能已被多次更新。但是，保证在`dispatch()`开始之前注册的所有用户在退出时都将被调用最新状态。

它是一个低级别的 API。最有可能的是，不是直接使用，而是使用 React（或其他）绑定。如果您通常使用回调作为响应状态更改的挂钩，则可能需要[编写自定义`observeStore`实用程序](https://github.com/reactjs/redux/issues/303#issuecomment-125184409)。`Store`也是一个[`Observable`](https://github.com/zenparsing/es-observable)，所以你可以用像 [RxJS ](https://github.com/ReactiveX/RxJS)这样的库进行更改`subscribe`。

要取消订阅更改侦听器，请调用返回`subscribe`的函数。

#### 参数

\1. listener (Function): 调用任何时候调用的回调，并且状态树可能已经改变。 您可以在此回调中调用`getState()`来读取当前状态树。 期望商店的 reducer 是一个纯函数是合理的，所以你可以比较一下在状态树中的一些深层路径的参考，以了解它的值是否已经改变。

##### 返回

（*Function*）：取消订阅更改侦听器的功能。

##### 示例

```javascript
function select(state) {
  return state.some.deep.property
}

let currentValue
function handleChange() {
  let previousValue = currentValue
  currentValue = select(store.getState())

  if (previousValue !== currentValue) {
    console.log(
      'Some deep nested property changed from',
      previousValue,
      'to',
      currentValue
    )
  }
}

let unsubscribe = store.subscribe(handleChange)
unsubscribe()
```

### `replaceReducer(nextReducer)`

替换存储当前使用的减速器来计算状态。

它是一个高级 API 。 如果您的应用实现代码拆分，并且您想要动态加载某些减速器，则可能需要此操作。 如果您为 Redux 实施热重新加载机制，则可能还需要此操作。

#### 参数

\2. `reducer`（*函数*）下一个用于存储的减速器。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18