# 储存（Store）

- 贡献者1人

  

在前面的章节中，我们定义了代表“发生了什么”的事实的动作以及根据这些动作更新状态的减法器。

该**存储**是带来他们的对象。该存储有以下责任：

- 保持应用程序状态;

- 允许通过`getState()`进入状态;

- 允许状态通过`dispatch(action)`更新;

- 通过`subscribe(listener)`注册接听器;

- 通过`subscribe(listener)`返回的函数处理注销监听器。

重要的是要注意，你将只有一个存储在 Redux 应用程序中。当你想拆分你的数据处理逻辑时，你将使用 reducer 组合而不是许多存储。

如果您有减速器，创建存储很容易。在前面的章节中，我们曾使用`combineReducers()`将几个 reducer 合并为一个。我们现在将它导入并传递它给`createStore()`。

```javascript
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```

您可以选择指定初始状态`createStore()`作为第二个参数。这对于保证客户端的状态与服务器上运行的 Redux 应用程序的状态相匹配非常有用。

```javascript
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

## 调度操作

现在我们已经创建了一个存储，让我们来验证我们的程序的作品！即使没有任何 UI，我们也可以测试更新逻辑。

```javascript
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// Log the initial state
console.log(store.getState())

// Every time the state changes, log it
// Note that subscribe() returns a function for unregistering the listener
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// Dispatch some actions
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// Stop listening to state updates
unsubscribe()
```

你可以看到这是如何导致存储的状态发生变化的：

![img](https://ask.qcloudimg.com/http-save/devdocs/y13a5pg9bt.png)

在我们开始编写 UI 之前，我们指定了我们的应用程序的行为。我们不会在本教程中做到这一点，但是现在您可以为您的减速器和动作创建者编写测试。你不需要嘲笑任何东西，因为它们只是纯粹的功能。调用他们，并就他们返回的内容作出断言。

## 源代码

#### `index.js`

```javascript
import { createStore } from 'redux'
import todoApp from './reducers'

let store = createStore(todoApp)
```

## 下一步

在为我们的待办事项应用程序创建用户界面之前，我们将绕路看看数据如何在 Redux 应用程序中流动。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18