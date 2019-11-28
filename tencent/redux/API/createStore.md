# createStore()

- 贡献者1人

  

创建一个 Redux 存储，用于存储应用程序的完整状态树。

应用中只能有一个存储。

#### 参数

1. `reducer` *（Function）*：给定当前状态树和要处理的动作的返回下一个状态树的减法函数。
2. `preloadedState` *（任何）*：初始状态。您可以选择将其指定为通用应用程序中的服务器状态，或恢复先前序列化的用户会话。如果您使用`combineReducers`它创建`reducer`，则它必须是一个与传递给它的键具有相同形状的普通对象。否则，你可以自由地传递任何你`reducer`能理解的东西。
3. `enhancer` *（功能）*：存储增强器。您可以选择指定它来增强存储的第三方功能，例如中间件，时间旅行，持久性等。Redux附带的唯一存储增强器是`applyMiddleware()`。

#### 返回

（*Store*）：保存应用程序完整状态的对象。改变其状态的唯一方法是调度行动。您也可以订阅其状态更改以更新 UI。

#### 示例

```javascript
import { createStore } from 'redux'

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}

let store = createStore(todos, ['Use Redux'])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
})

console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```

#### 提示

- 不要在应用程序中创建多个存储！相反，使用多个`combineReducers`创建单个根减速器。
- 您需要选择状态格式。你可以使用普通对象或类似 [Immutable ](http://facebook.github.io/immutable-js/)的东西。如果您不确定，请从简单对象开始。
- 如果你的状态是一个普通的对象，确保你永远不会改变它！例如，不要从你的 reducer 中返回类似`Object.assign(state, newData)`的东西，而是返回`Object.assign({}, state, newData)`。这样你不会覆盖以前的`state`。如果启用对象扩展运算符提议，也可以编写`return { ...state, ...newData }`。
- 对于在服务器上运行的通用应用程序，请为每个请求创建一个存储实例，以便将它们隔离。将一些数据提取动作分发给商店实例，并等待它们在服务器上呈现应用程序之前完成。
- 当一个存储被创建时，Redux 会向您的减速器分派一个虚拟操作，以使用初始状态填充存储。你并不是想直接处理虚拟行为。只要记住你的减速器应该返回某种初始状态，如果给它的状态作为第一个`undefined`参数，并且你全部设置了。
- 要应用多个存储增强器，您可以使用`compose()`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18