# combineReducers()

- 贡献者1人

  

随着您的应用程序变得越来越复杂，您需要将您的减少功能分解为单独的功能，每个功能都管理着独立的状态部分。

`combineReducers`辅助函数将一个对象，其值是从不同的减少函数转换成你可传递到`createStore`的一个单一减小函数。

生成的缩减器调用每个子缩减器，并将其结果收集到单个状态对象中。**状态对象的形状与传递的reducers键相匹配** 。

因此，状态对象将如下所示：

```javascript
{
  reducer1: ...
  reducer2: ...
}
```

您可以通过对传递对象中的减速器使用不同的键来控制状态键名称。例如，您可能会调用`combineReducers({ todos: myTodosReducer, counter: myCounterReducer })`状态为`{ todos, counter }`。

一种流行的惯例是在状态切片管理后命名缩减器，因此您可以使用 ES6 属性速记符号：`combineReducers({ counter, todos })`。这相当于写`combineReducers({ counter: counter, todos: todos })`。

> Flux 用户注意事项
>
> 此功能可帮助您组织 Reducer 管理自己的状态片，就像您将拥有不同的 Flux Stores 来管理不同的状态一样。借助 Redux，只有一个存储，但`combineReducers`可以帮助您在减速器之间保持相同的逻辑分工。

#### 参数

1. `reducers`（*Object*）：一个对象，其值与需要合并为一个的不同缩减函数相对应。请参阅下面的注释，了解减速器必须遵循的每个规则。之前的文档建议使用 ES6 `import * as reducers`语法来获取 reducer 对象。这是很多混淆的原因，这就是为什么我们现在推荐输出使用从`reducers/index.js`替换成的`combineReducers()`从而获得的单个减速器。下面是一个例子。返回值（*函数*）：调用`reducers`对象内每个 Reducer 内的每个 Reducer，并构造一个具有相同形状的状态对象。注意这个函数有点顽固，并倾向于帮助初学者避免常见的陷阱。这就是为什么它试图强制执行一些你不必遵循的规则，如果你手动编写根 reducer。传递给`combineReducers`的任何 reducer 必须满足这些规则：

\2. 对于任何未被识别的动作，它必须将给定的`state`返回值作为第一个参数。

- 它永远不会返回`undefined`。通过一个早期的`return`语句很容易做错，所以如果你这样做会引发`combineReducers`错误，而不是让错误在其他地方出现。

- 如果它的`state`给定`undefined`的话，它必须返回这个特定减速器的初始状态。根据之前的规则，初始状态也不能是`undefined`。使用ES6可选参数语法指定它很方便，但您也可以显式检查第一个参数是否存在`undefined`。

尽管`combineReducers`试图检查你的减速器是否符合这些规则中的一部分，但你应该记住它们，并尽力遵循它们。`combineReducers`通过传递`undefined`给他们来检查你的减速器；即使您指定了初始状态，也会执行`Redux.createStore(combineReducers(...), initialState)`操作。因此，即使您从未打算让他们实际接收在您的代码`undefined`，您也**必须**确保您的减速器在接收`undefined`状态时正常工作。

#### 示例

#### `reducers/todos.js`

```javascript
export default function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}
```

#### `reducers/counter.js`

```javascript
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

#### `reducers/index.js`

```javascript
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'

export default combineReducers({
  todos,
  counter
})
```

#### `App.js`

```javascript
import { createStore } from 'redux'
import reducer from './reducers/index'

let store = createStore(reducer)
console.log(store.getState())
// {
//   counter: 0,
//   todos: []
// }

store.dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
})
console.log(store.getState())
// {
//   counter: 0,
//   todos: [ 'Use Redux' ]
// }
```

#### 提示

- 这个帮助只是一个方便！你可以写你自己[工作方式不同](https://github.com/acdlite/reduce-reducers)的`combineReducers`，甚至手工组装从子减速状态对象，并明确写入根还原功能，就像你会写任何其他函数。

- 您可以调用`combineReducers`减速器层次结构的任何级别。它不必在顶部发生。事实上，您可能会再次使用它来将太复杂的子减速器拆分为独立的孙减速器，等等。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18