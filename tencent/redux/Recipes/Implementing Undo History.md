# 实现撤消历史操作( Implementing Undo History )

- 贡献者1人

  

在应用程序中构建撤消和重做功能传统上需要开发人员的有意识的努力。这对于传统的MVC框架来说不是一个简单的问题，因为您需要通过克隆所有相关模型来跟踪每个过去的状态。另外，您需要注意撤销堆栈，因为用户启动的更改应该是可撤销的。

这意味着在 MVC 应用程序中实现撤消和重做通常会迫使您重写应用程序的某些部分以使用特定的数据突变模式，如 [Command ](https://en.wikipedia.org/wiki/Command_pattern)。

但是，使用 Redux 可以轻松实现撤消历史记录。这有三个原因：

- 没有多个模型 - 只是您想跟踪的状态子树。

- 状态已经是不可变的，突变已经被描述为离散行为，这与离散堆栈智能模型很接近。

- 减速器`(state, action) => state`签名使得实现通用的“减速器增强器”或“更高阶的减速器”是自然的。它们的一些功能，可以让您的缩减器在增加功能的同时保留其签名。撤消历史就是这种情况。

在继续之前，确保你已经完成了基础知识教程，并很好地理解了减速器的组成。本教程将基于基础教程中介绍的示例进行构建。

在这个教程的第一部分中，我们将解释使 Undo 和 Redo 可以以通用方式实现的基本概念。

在本配方的第二部分中，我们将演示如何使用 [Redux Undo ](https://github.com/omnidan/redux-undo)软件包，该软件包可以提供此功能。

![img](https://ask.qcloudimg.com/http-save/devdocs/3p0pj9ru9e.gif)

## 了解撤消历史操作

### 设计状态形状

撤消历史记录也是您应用程序状态的一部分，我们没有理由对其采取不同的处理方式。无论状态类型如何随时间变化，当您执行撤消和重做时，您都希望在不同的时间点跟踪此状态的**历史记录**。

例如，计数器应用的状态形状可能如下所示：

```javascript
{
  counter: 10
}
```

If we wanted to implement Undo and Redo in such an app, we'd need to store more state so we can answer the following questions:

- 还有什么可以撤消或重做的吗？

- 现状是什么？

- 撤消堆栈中的过去（和未来）状态是什么？

建议我们改变状态形态以回答这些问题是合理的：

```javascript
{
  counter: {
    past: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    present: 10,
    future: []
  }
}
```

现在，如果用户按下“撤消”，我们希望它改变成过去：

```javascript
{
  counter: {
    past: [0, 1, 2, 3, 4, 5, 6, 7, 8],
    present: 9,
    future: [10]
  }
}
```

还有：

```javascript
{
  counter: {
    past: [0, 1, 2, 3, 4, 5, 6, 7],
    present: 8,
    future: [9, 10]
  }
}
```

当用户按下“重做”时，我们想要向后退一步：

```javascript
{
  counter: {
    past: [0, 1, 2, 3, 4, 5, 6, 7, 8],
    present: 9,
    future: [10]
  }
}
```

最后，如果用户在撤销堆栈中间执行一个操作（例如减少计数器），我们将放弃已存在的未来操作：

```javascript
{
  counter: {
    past: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    present: 8,
    future: []
  }
}
```

这里有趣的部分是，我们是否想要保留数字，字符串，数组或对象的撤消堆栈并不重要。结构将始终如一：

```javascript
{
  counter: {
    past: [0, 1, 2],
    present: 3,
    future: [4]
  }
}
{
  todos: {
    past: [
      [],
      [{ text: 'Use Redux' }],
      [{ text: 'Use Redux', complete: true }]
    ],
    present: [
      { text: 'Use Redux', complete: true },
      { text: 'Implement Undo' }
    ],
    future: [
      [
        { text: 'Use Redux', complete: true },
        { text: 'Implement Undo', complete: true }
      ]
    ]
  }
}
```

一般来说，它看起来像这样：

```javascript
{
  past: Array<T>,
  present: T,
  future: Array<T>
}
```

是否保持单一的顶级历史也取决于我们：

```javascript
{
  past: [
    { counterA: 1, counterB: 1 },
    { counterA: 1, counterB: 0 },
    { counterA: 0, counterB: 0 }
  ],
  present: { counterA: 2, counterB: 1 },
  future: []
}
```

或者许多细粒度的历史记录，用户可以独立地撤消和重做其中的操作：

```javascript
{
  counterA: {
    past: [1, 0],
    present: 2,
    future: []
  },
  counterB: {
    past: [0],
    present: 1,
    future: []
  }
}
```

稍后我们将看到我们采取的方法让我们如何选择Undo和Redo需要的粒度。

### 设计算法

无论具体的数据类型如何，撤消历史状态的形状都是相同的：

```javascript
{
  past: Array<T>,
  present: T,
  future: Array<T>
}
```

让我们通过算法来讨论如何操作上面描述的状态。我们可以定义两种操作来处理这个状态：`UNDO`和`REDO`。在我们的 reducer 中，我们将执行以下步骤来处理这些操作：

#### 处理撤消

- 从中删除**最后***一个*元素`past`。

- 将我们在前一步中移除的元素设置为`present`。

- 插入旧`present`状态的`future`**开始**处。

#### 处理重做

- 从中删除**第一个**元素`future`。

- 将我们在前一步中移除的元素设置为`present`。

- 在`past`**结尾**插入旧`present`的状态。

#### 处理其他行为

- 在`present`最后插入`past`。

- `present`处理完操作后，将其设置为新状态。

- 清除`future`。

### 第一次尝试：编写一个减速器

```javascript
const initialState = {
  past: [],
  present: null, // (?) How do we initialize the present?
  future: []
}

function undoable(state = initialState, action) {
  const { past, present, future } = state

  switch (action.type) {
    case 'UNDO':
      const previous = past[past.length - 1]
      const newPast = past.slice(0, past.length - 1)
      return {
        past: newPast,
        present: previous,
        future: [present, ...future]
      }
    case 'REDO':
      const next = future[0]
      const newFuture = future.slice(1)
      return {
        past: [...past, present],
        present: next,
        future: newFuture
      }
    default:
      // (?) How do we handle other actions?
      return state
  }
}
```

这种实现不可用，因为它遗漏了三个重要问题：

- 我们从哪里获得初始`present`状态？我们事先并不知道。

- 我们在哪里给外部的行动作出反应，将`present`保存到`past`？

- 我们如何对`present`状态的控制委托给自定义缩减器？

reducer不是正确的抽象，但我们非常接近。

### 认识减速器增强器

您可能熟悉[更高阶的函数](https://en.wikipedia.org/wiki/Higher-order_function)。如果您使用 React，您可能熟悉[更高阶的组件](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)。这是一种适用于减速器的相同模式的变体。

**减速增强**（或更高阶减速）是一个函数，需要一个减速，并返回一个新减速机，它能够处理新的动作，或者持有更多的状态，委托控制减速因为它没有理解行动。这不是一种新模式 - 从技术上讲，`combineReducers()`也是减速器增强器，因为它需要减速器并返回新的减速器。

不做任何事情的减速器增强器看起来像这样：

```javascript
function doNothingWith(reducer) {
  return function (state, action) {
    // Just call the passed reducer
    return reducer(state, action)
  }
}
```

结合其他减速器的减速器增强器可能如下所示：

```javascript
function combineReducers(reducers) {
  return function (state = {}, action) {
    return Object.keys(reducers).reduce((nextState, key) => {
      // Call every reducer with the part of the state it manages
      nextState[key] = reducers[key](state[key], action)
      return nextState
    }, {})
  }
}
```

### 第二次尝试：编写一个 Reducer 增强器

现在我们对减速器增强器有了更好的理解，我们可以看到，`undoable`本来应该是如下形式：

```javascript
function undoable(reducer) {
  // Call the reducer with empty action to populate the initial state
  const initialState = {
    past: [],
    present: reducer(undefined, {}),
    future: []
  }

  // Return a reducer that handles undo and redo
  return function (state = initialState, action) {
    const { past, present, future } = state

    switch (action.type) {
      case 'UNDO':
        const previous = past[past.length - 1]
        const newPast = past.slice(0, past.length - 1)
        return {
          past: newPast,
          present: previous,
          future: [present, ...future]
        }
      case 'REDO':
        const next = future[0]
        const newFuture = future.slice(1)
        return {
          past: [...past, present],
          present: next,
          future: newFuture
        }
      default:
        // Delegate handling the action to the passed reducer
        const newPresent = reducer(present, action)
        if (present === newPresent) {
          return state
        }
        return {
          past: [...past, present],
          present: newPresent,
          future: []
        }
    }
  }
}
```

现在我们可以将任何减速器包装到`undoable`减速器增强器中，以教会它对`UNDO`和`REDO`做出反应采取行动。

```javascript
// This is a reducer
function todos(state = [], action) {
  /* ... */
}

// This is also a reducer!
const undoableTodos = undoable(todos)

import { createStore } from 'redux'
const store = createStore(undoableTodos)

store.dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
})

store.dispatch({
  type: 'ADD_TODO',
  text: 'Implement Undo'
})

store.dispatch({
  type: 'UNDO'
})
```

有一个重要的问题：当你检索`.present`时，你需要记住追加到当前状态。您也可以检查`.past.length`和`.future.length`确定是分别启用还是禁用“撤消”和“重做”按钮。

您可能听说Redux受[Elm Architecture](https://github.com/evancz/elm-architecture-tutorial/)影响。这个例子与[elm-undo-redo包](http://package.elm-lang.org/packages/TheSeamau5/elm-undo-redo/2.0.0)非常相似，不应该让人吃惊。

## 使用Redux撤消

这是非常丰富的，但我们不能只是放下一个目录，并使用它而不是实现`undoable`？我们当然可以！结识[Redux Undo](https://github.com/omnidan/redux-undo)，它是一个为Redux树的任何部分提供简单撤销和重做功能的库。

在这部分教程中，您将学习如何使 Todo List 示例进行撤消。您可以在[`todos-with-undo`Redux附带](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)的[示例中](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)找到此教程的完整源代码。

### 安装

首先，你需要运行

```javascript
npm install --save redux-undo
```

这将安装提供`undoable`减速器增强器的软件包。

### 封装减速机

你需要用你的`undoable`功能来包装你想要增强的减速器。例如，如果您从专用文件`todos`导出 reducer，则需要将其更改为`undoable()，`使用您写入的 reducer 导出调用结果：

#### `reducers/todos.js`

```javascript
import undoable, { distinctState } from 'redux-undo'

/* ... */

const todos = (state = [], action) => {
  /* ... */
}

const undoableTodos = undoable(todos, {
  filter: distinctState()
})

export default undoableTodos
```

`distinctState()`过滤器用于忽略不会导致状态更改的操作。还有[许多其他选项可](https://github.com/omnidan/redux-undo#configuration)用于配置可撤销缩减器，例如设置“撤消”和“重做”操作的操作类型。

请注意，`combineReducers()`通话将保持原样，但`todos`减速器现在将引用通过Redux撤消增强的减速器：

#### `reducers/index.js`

```javascript
import { combineReducers } from 'redux'
import todos from './todos'
import visibilityFilter from './visibilityFilter'

const todoApp = combineReducers({
  todos,
  visibilityFilter
})

export default todoApp
```

您可以在 Reducer 组合层次结构`undoable`的任何级别中包装一个或多个Reducer。我们选择换行`todos`而不是顶级组合缩减器，以便更改`visibilityFilter`并不会反映在撤消历史记录中。

### 更新选择器

现在`todos`状态的部分如下所示：

```javascript
{
  visibilityFilter: 'SHOW_ALL',
  todos: {
    past: [
      [],
      [{ text: 'Use Redux' }],
      [{ text: 'Use Redux', complete: true }]
    ],
    present: [
      { text: 'Use Redux', complete: true },
      { text: 'Implement Undo' }
    ],
    future: [
      [
        { text: 'Use Redux', complete: true },
        { text: 'Implement Undo', complete: true }
      ]
    ]
  }
}
```

这意味着你需要访问你的状态`state.todos.present`而不是`state.todos`：

#### `containers/VisibleTodoList.js`

```javascript
const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos.present, state.visibilityFilter)
  }
}
```

### 添加按钮

现在，您只需添加用于撤消和重做操作的按钮即可。

首先，`UndoRedo`为这些按钮创建一个新的容器组件。我们不破坏将表示部分拆分为单独的文件，因为它非常小：

#### `containers/UndoRedo.js`

```javascript
import React from 'react'

/* ... */

let UndoRedo = ({ canUndo, canRedo, onUndo, onRedo }) => (
  <p>
    <button onClick={onUndo} disabled={!canUndo}>
      Undo
    </button>
    <button onClick={onRedo} disabled={!canRedo}>
      Redo
    </button>
  </p>
)
```

您将使用`connect()`从[阵营终极版](https://github.com/reactjs/react-redux)生成一个容器组件。要确定是否启用撤消和重做按钮，您可以检查`state.todos.past.length`和`state.todos.future.length`。您不需要编写动作创建器来执行撤消和重做，因为 Redux 撤销提供了如下代码：

#### `containers/UndoRedo.js`

```javascript
/* ... */

import { ActionCreators as UndoActionCreators } from 'redux-undo'
import { connect } from 'react-redux'

/* ... */

const mapStateToProps = state => {
  return {
    canUndo: state.todos.past.length > 0,
    canRedo: state.todos.future.length > 0
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onUndo: () => dispatch(UndoActionCreators.undo()),
    onRedo: () => dispatch(UndoActionCreators.redo())
  }
}

UndoRedo = connect(
  mapStateToProps,
  mapDispatchToProps
)(UndoRedo)

export default UndoRedo
```

现在您可以将`UndoRedo`组件添加到`App`组件中：

#### `components/App.js`

```javascript
import React from 'react'
import Footer from './Footer'
import AddTodo from '../containers/AddTodo'
import VisibleTodoList from '../containers/VisibleTodoList'
import UndoRedo from '../containers/UndoRedo'

const App = () => (
  <div>
    <AddTodo />
    <VisibleTodoList />
    <Footer />
    <UndoRedo />
  </div>
)

export default App
```

这就是我们要介绍的方式！运行`npm install`和`npm start`并在[示例文件夹中](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)尝试一下！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18