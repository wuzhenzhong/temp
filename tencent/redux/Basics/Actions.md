# 操作（Actions）

- 贡献者1人

  

首先，我们来定义一些操作。

**操作**是将数据从应用程序发送到商店的信息的有效载荷。他们是商店*唯一*的信息来源。您使用它们将它们发送到商店`store.dispatch()`。

以下是一个示例操作，表示添加新的待办事项：

[纠错](javascript:;)

```javascript
const ADD_TODO = 'ADD_TODO'
{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

操作是纯 JavaScript 对象。操作必须具有`type`指示正在执行的操作类型的属性。类型通常应该定义为字符串常量。一旦您的应用程序足够大，您可能需要将它们移动到单独的模块中。

```javascript
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

> Note on Boilerplate You don't have to define action type constants in a separate file, or even to define them at all. For a small project, it might be easier to just use string literals for action types. However, there are some benefits to explicitly declaring constants in larger codebases. Read Reducing Boilerplate for more practical tips on keeping your codebase clean.

除此`type`之外，动作对象的结构真的取决于你。如果您有兴趣，请查看 [Flux 标准行动部门](https://github.com/acdlite/flux-standard-action)，了解如何构建[行动](https://github.com/acdlite/flux-standard-action)的建议。

我们将添加一个动作类型来描述用户将待办事项勾选完成。我们提到一个特定的待办事项，`index`因为我们将它们存储在一个数组中。在真正的应用程序中，每次创建新内容时都要生成唯一的ID。

```javascript
{
  type: TOGGLE_TODO,
  index: 5
}
```

尽可能在每个操作中传递尽可能少的数据是个好主意。例如，最好通过`index`比整个待办事项对象。

最后，我们将添加一个更改动作类型来更改当前可见的待办事项。

```javascript
{
  type: SET_VISIBILITY_FILTER,
  filter: SHOW_COMPLETED
}
```

## 动作创作者

**动作创作者**就是那种创造动作的功能。很容易混淆术语“动作”和“动作创造者”，因此尽量使用适当的术语。

在 Redux 动作创建者只需返回一个动作：

```javascript
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

这使得它们便于携带并且易于测试。

在[传统的 Flux 中](http://facebook.github.io/flux)，动作创建者在调用时经常会触发一次调度，如下所示：

```javascript
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    text
  }
  dispatch(action)
}
```

在Redux中，情况*并非*如此。

相反，要实际启动一次调度，请将结果传递给该`dispatch()`函数：

```javascript
dispatch(addTodo(text))
dispatch(completeTodo(index))
```

或者，您可以创建一个自动调度的**绑定动作创建器**：

```javascript
const boundAddTodo = text => dispatch(addTodo(text))
const boundCompleteTodo = index => dispatch(completeTodo(index))
```

现在你可以直接调用他们了：

```javascript
boundAddTodo(text)
boundCompleteTodo(index)
```

`dispatch()`函数可以直接从商店的访问`store.dispatch()`，但更可能你会使用像一个助手来访问它[的反应，终极版](http://github.com/gaearon/react-redux)的`connect()`。您可以使用`bindActionCreators()`自动将许多动作创建者绑定到一个`dispatch()`函数。

动作创作者也可以是异步的并具有副作用。您可以阅读高级教程中的异步操作，以了解如何处理AJAX响应并将动作创建者编写为异步控制流。在完成基础教程之前，不要直接跳到异步操作，因为它涵盖了高级教程和异步操作的先决条件的其他重要概念。

## 源代码

### `actions.js`

```javascript
/*
 * action types
 */

export const ADD_TODO = 'ADD_TODO'
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

/*
 * other constants
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}

/*
 * action creators
 */

export function addTodo(text) {
  return { type: ADD_TODO, text }
}

export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}
```

## 下一步

现在让我们定义一些 reducer 来指定当您分派这些操作时状态如何更新！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18