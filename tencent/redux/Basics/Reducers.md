# 减速器（Reducers）

- 贡献者1人

  

操作描述*事情发生*的事实，但不指定应用程序的状态如何响应。这是减速机的工作。

## 设计状态形状

在 Redux 中，所有应用程序状态都存储为单个对象。在编写任何代码之前考虑它的形状是个好主意。什么是你的应用程序状态作为对象的最小表示？

对于我们的待办应用程序，我们想要存储两件不同的事情：

- 当前选定的可见性过滤器;

- 实际的待办事项列表。

您经常会发现，您需要在状态树中存储一些数据以及一些 UI 状态。这很好，但要尽量将数据与 UI 状态分开。

```javascript
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

> 关于关系的注意事项
>
> 在更复杂的应用程序中，您将希望不同的实体相互引用。我们建议您尽可能保持您的状态正常化，无需嵌套。将存储有ID的对象中的每个实体都保存为键，并使用ID从其他实体或列表中引用它。将应用程序的状态视为数据库。这种方法在 [normalizr ](https://github.com/paularmstrong/normalizr)文档中有详细描述。例如，在一个真实的应用程序中保持状态`todosById: { id -> todo }`和`todos: array<id>`在状态内是更好的主意，但我们保持简单的例子。

## 处理操作

现在我们已经决定了我们的状态对象，我们准备为它编写一个 reducer。reducer 是一个纯函数，它接受之前的状态和一个动作，并返回下一个状态。

```javascript
(previousState, action) => newState
```

它被称为 reducer，因为它是你传递给[`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)的函数的类型。减速机保持纯净非常重要。减速机内**永远**不应该做的事情：

- 改变论点;

- 执行 API 调用和路由转换等副作用;

- 调用非纯函数，例如`Date.now()`或`Math.random()`。

我们将探讨如何在高级演练中执行副作用。现在，请记住减速器必须是纯粹的。**给定相同的参数，它应该计算下一个状态并返回它。没有惊喜。没有副作用。没有 API 调用。没有突变。只是一个计算。**

有了这个，让我们开始写我们的减速机，逐渐地教它来理解我们之前定义的动作。

我们将首先指定初始状态。Redux 首次用`undefined`状态调用我们的减速器。这是我们返回应用程序初始状态的机会：

```javascript
import { VisibilityFilters } from './actions'

const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
}

function todoApp(state, action) {
  if (typeof state === 'undefined') {
    return initialState
  }

  // For now, don't handle any actions
  // and just return the state given to us.
  return state
}
```

一个巧妙的技巧是使用 [ES6 默认参数语法](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/default_parameters)以更紧凑的方式编写它：

```javascript
function todoApp(state = initialState, action) {
  // For now, don't handle any actions
  // and just return the state given to us.
  return state
}
```

现在让我们来处理`SET_VISIBILITY_FILTER`。它所需要做的就是改变`visibilityFilter`状态。简单来说：

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

注意：

1. **我们不会改变** **state\****。**我们创建一个**[**Object.assign()**](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)**副本。Object.assign(state, { visibilityFilter: action.filter })也是错的：它会改变第一个参数。您**必须**提供一个空对象作为第一个参数。您也可以启用对象扩展运算符提议`{ ...state, ...newState }`来代替写入。

**2. 我们回到以前state的default情况。**对于任何未知的操作返回`state`前一个都很重要。

> 注意`Object.assign`
>
> [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)是 ES6 的一部分，但大多数浏览器尚未实现。您需要使用 polyfill，[Babel 插件](https://www.npmjs.com/package/babel-plugin-transform-object-assign)或来自其他像`_.assign()`库的帮助程序。
>
> 关于`switch`和 Boilerplate
>
> `switch`说明声明*不是*真正的样板。Flux 真正的模板是概念性的：需要发布更新，需要使用调度程序注册 Store，需要 Store 作为对象（以及在需要通用应用程序时出现的复杂情况）。Redux 通过使用纯缩减器而不是事件发射器来解决这些问题。
>
> 不幸的是，许多人仍然根据是否使用`switch`文档中的语句来选择框架。如果你不喜欢`switch`，你可以使用自定义`createReducer` 函数接受处理程序映射，如“简化样板”所示。

## 处理更多操作

我们还有两个行动来处理！就像我们所做的`SET_VISIBILITY_FILTER`那样，我们将导入`ADD_TODO`和`TOGGLE_TODO`操作，然后扩展我们的 reducer 来处理`ADD_TODO`。

```javascript
import { VisibilityFilters, ADD_TODO, TOGGLE_TODO } from './actions'

...

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    default:
      return state
  }
}
```

就像以前一样，我们绝不会直接写入`state`或其字段，而是返回新的对象。新`todos`的等于旧的`todos`与最后一个新的项目连接在一起。新鲜的待办事项是使用行动中的数据构建的。

最后，`TOGGLE_TODO`处理程序的实现不应该是一个完全惊喜：

```javascript
case TOGGLE_TODO:
  return Object.assign({}, state, {
    todos: state.todos.map((todo, index) => {
      if (index === action.index) {
        return Object.assign({}, todo, {
          completed: !todo.completed
        })
      }
      return todo
    })
  })
```

因为我们想更新数组中的某个特定项目而不使用突变，所以我们必须创建一个新数组，其中除了索引处的项目外，其他项目都是相同的。如果你发现自己经常编写这样的操作，最好使用像[不可变性助手](https://github.com/kolodny/immutability-helper)，[updeep ](https://github.com/substantial/updeep)[的助手](https://github.com/kolodny/immutability-helper)，或者像 [Immutable ](http://facebook.github.io/immutable-js/)这样的库，它具有对深度更新的本机支持。请记住，除非您先复制它，否则绝对不要指定任何`state`内容。

## 分裂减速器

这是我们的代码。它比较冗长：

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: state.todos.map((todo, index) => {
          if (index === action.index) {
            return Object.assign({}, todo, {
              completed: !todo.completed
            })
          }
          return todo
        })
      })
    default:
      return state
  }
}
```

有没有办法让它更容易理解？它看起来像`todos`和`visibilityFilter`完全独立更新。有时状态字段彼此依赖，需要更多的考虑，但在我们的例子中，我们可以很容易地将更新`todos`分成单独的函数：

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: todos(state.todos, action)
      })
    default:
      return state
  }
}
```

请注意，`todos`也接受`state`- 但它是一个数组！现在`todoApp`只是给它一个管理状态的切片，并且`todos`知道如何更新该切片。**这就是所谓的** **减速器组成，它是构建Redux应用程序的基本模式。**

我们来更多地探讨减速器的组成。我们是否也可以提取减速器管理`visibilityFilter`？我们可以。

在我们的导入下面，让我们使用[ES6 Object Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)来声明`SHOW_ALL`：

```javascript
const { SHOW_ALL } = VisibilityFilters
```

然后：

```javascript
function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}
```

现在我们可以将主 Reducer 重写为一个函数，该函数调用管理该状态部分的 reducer，并将它们合并为一个对象。它也不需要知道完整的初始状态了。刚开始`undefined`给子减压器返回初始状态就足够了。

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

**请注意，这些减速器中的每一个都在管理其全部状态的一部分。** **每个 reducer 的state参数都不相同，并且对应于它所管理的状态的一部分。**

这已经很好看了！当应用程序较大时，我们可以将减速器拆分为单独的文件并使其完全独立并管理不同的数据域。

最后，Redux 提供了一个名为`combineReducers()`的实用程序，它执行与`todoApp`上面目前相同的样板逻辑。在它的帮助下，我们可以像这样重写`todoApp`：

```javascript
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

请注意，这相当于：

```javascript
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

你也可以给他们不同的键，或以不同的方式调用函数。这两种写联合减速器的方法是等价的：

```javascript
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

所有的`combineReducers()`功能都会生成一个函数，**根据其按键选择所选状态的切片**并将其结果再次组合到单个对象中。[这不是魔术。](https://github.com/reactjs/redux/issues/428#issuecomment-129223274)和其他减速器一样，如果向其提供的所有减速器不改变状态，则`combineReducers()`不会创建新的对象。

> ES6 精明用户注意
>
> 因为`combineReducers`需要一个对象，所以我们可以将所有顶级缩减器放入一个单独的文件，`export`每个`import * as reducers`缩减器函数中，并用它们作为对象，将它们的名称作为键： import { combineReducers } from 'redux' import * as reducers from './reducers' const todoApp = combineReducers(reducers)
>
> 因为`import *`仍然是新的语法，所以我们不再在文档中使用它来避免[混淆](https://github.com/reactjs/redux/issues/428#issuecomment-129223274)，但是您可能会在某些社区示例中遇到。

## 源代码

#### `reducers.js`

```javascript
import { combineReducers } from 'redux'
import {
  ADD_TODO,
  TOGGLE_TODO,
  SET_VISIBILITY_FILTER,
  VisibilityFilters
} from './actions'
const { SHOW_ALL } = VisibilityFilters

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

## 下一步

接下来，我们将探讨如何创建一个 Redux 存储来保存状态，并在分派操作时负责调用您的 reducer。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18