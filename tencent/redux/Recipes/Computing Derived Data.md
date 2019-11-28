# 计算派生数据( Computing Derived Data )

- 贡献者1人

  

[Reselect](https://github.com/reactjs/reselect)是用于创建memoized，可组合**选择器**功能的简单库。重新选择选择器可用于有效地计算来自Redux商店的派生数据。

### Memoized 选择器的动机

让我们重温一下 Todos List 的例子：

#### `containers/VisibleTodoList.js`

```javascript
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

在上面的例子中，`mapStateToProps`调用`getVisibleTodos`计算`todos`。这很好，但是有一个缺点：`todos`每次组件更新时都会计算。如果状态树很大，或者计算开销很大，则在每次更新时重复计算都可能导致性能问题。重新选择可以帮助避免这些不必要的重新计算。

### 创建一个 Memoized 选择器

我们想用一个memoized选择器`todos`替换`getVisibleTodos`，选择器在状态树的其他（不相关的），如`state.todos`或`state.visibilityFilter`发生变化时重新计算值的何时

重新选择为`createSelector`提供了创建 memoized 选择器的功能。`createSelector`以一组输入选择器和一个变换函数作为参数。如果Redux状态树导致输入选择器值发生变化的方式进行变异，则选择器将使用输入选择器的值作为参数调用其变换函数并返回结果。如果输入选择器的值与先前对选择器的调用相同，则它将返回先前计算的值，而不是调用变换函数。

让我们定义一个名为 memoized 的选择器`getVisibleTodos`来替换上面的非 memoized 版本：

#### `selectors/index.js`

```javascript
import { createSelector } from 'reselect'

const getVisibilityFilter = state => state.visibilityFilter
const getTodos = state => state.todos

export const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

在上面的例子中，`getVisibilityFilter`和`getTodos`输入选择器。它们被创建为普通的非memoized选择器函数，因为它们不转换他们选择的数据`getVisibleTodos`。另一方面是一个选择器。它需要`getV`memoized`isibilityFilter`何和`getTodos`作为输入选择器，以及计算过滤待办事项列表的转换函数。

### 编写选择器

memoized 选择器本身可以是另一个 memoized 选择器的输入选择器。这里`getVisibleTodos`被用作一个选择器的输入选择器，它通过关键字进一步过滤待办事项：

```javascript
const getKeyword = state => state.keyword

const getVisibleTodosFilteredByKeyword = createSelector(
  [getVisibleTodos, getKeyword],
  (visibleTodos, keyword) =>
    visibleTodos.filter(todo => todo.text.indexOf(keyword) > -1)
)
```

### 将选择器连接到Redux存储

如果您使用的是 [React Redux ](https://github.com/reactjs/react-redux)，则可以在内部调用选择器作为常规函数 `mapStateToProps() `：

#### `containers/VisibleTodoList.js`

```javascript
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state)
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

### 在选择器中访问React道具

> 本节介绍了我们的应用程序的一个假设扩展，它允许它支持多个待办事项列表。请注意，完整实施此扩展需要对减少器，组件，操作等进行更改，这些操作与讨论的主题没有直接关系，为简洁起见，此处省略。

到目前为止，我们只看到选择器接收 Redux 商店状态作为参数，但选择器也可以接收道具。

这是一个`App`渲染三个`VisibleTodoList`组件的组件，每个组件都有一个`listId`prop：

#### `components/App.js`

```javascript
import React from 'react'
import Footer from './Footer'
import AddTodo from '../containers/AddTodo'
import VisibleTodoList from '../containers/VisibleTodoList'

const App = () => (
  <div>
    <VisibleTodoList listId="1" />
    <VisibleTodoList listId="2" />
    <VisibleTodoList listId="3" />
  </div>
)
```

每个`VisibleTodoList`容器应根据`listId`道具的值选择不同的状态片段，因此我们修改`getVisibilityFilter`和`getTodos`接受道具参数：

#### `selectors/todoSelectors.js`

```javascript
import { createSelector } from 'reselect'

const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter

const getTodos = (state, props) => state.todoLists[props.listId].todos

const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_COMPLETED':
        return todos.filter(todo => todo.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(todo => !todo.completed)
      default:
        return todos
    }
  }
)

export default getVisibleTodos
```

`props`可以从`mapStateToProps`传递给`getVisibleTodos`：

```javascript
const mapStateToProps = (state, props) => {
  return {
    todos: getVisibleTodos(state, props)
  }
}
```

所以现在`getVisibleTodos`有权访问`props`，并且一切似乎都正常工作。

**但存在一个问题！**

`getVisibleTodos`对`visibleTodoList`容器的多个实例使用选择器将无法正确记忆：

#### `containers/VisibleTodoList.js`

```javascript
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = (state, props) => {
  return {
    // WARNING: THE FOLLOWING SELECTOR DOES NOT CORRECTLY MEMOIZE
    todos: getVisibleTodos(state, props)
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

`createSelector`仅当创建的选择器与其前一组参数相同时，才会返回缓存的值。如果我们渲染之间交替`<VisibleTodoList` `listId="1"` `/>`和`<VisibleTodoList` `listId="2"` `/>`，共享选择器将接收之间交替`{listId: 1}`和`{listId: 2}`作为其`props`参数。这会导致参数在每次调用时都不相同，因此选择器将始终重新计算而不是返回缓存的值。我们将在下一节看到如何克服这个限制。

### 在多个组件上共享选择器

> 本节中的示例需要React Redux v4.3.0或更高版本

为了在多个`VisibleTodoList`组件之间共享选择器**并**保留备忘录，组件的每个实例都需要其自己的选择器专用副本。

让我们创建一个名为`makeGetVisibleTodos`的函数，`getVisibleTodos`每次调用时都会返回选择器的新副本：

#### `selectors/todoSelectors.js`

```javascript
import { createSelector } from 'reselect'

const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter

const getTodos = (state, props) => state.todoLists[props.listId].todos

const makeGetVisibleTodos = () => {
  return createSelector(
    [getVisibilityFilter, getTodos],
    (visibilityFilter, todos) => {
      switch (visibilityFilter) {
        case 'SHOW_COMPLETED':
          return todos.filter(todo => todo.completed)
        case 'SHOW_ACTIVE':
          return todos.filter(todo => !todo.completed)
        default:
          return todos
      }
    }
  )
}

export default makeGetVisibleTodos
```

我们还需要一种方法来让每个容器的实例访问它自己的私有选择器。`mapStateToProps`的参数`connect`可以帮助这个。

**如果** **提供的参数返回一个函数而不是一个对象，它将用于为容器的每个实例mapStateToProps创建一个单独的函数** **connect** **mapStateToProps** 。

在下面的例子中`makeMapStateToProps`创建一个新的`getVisibleTodos`选择器，并返回一个可独占访问新选择器的函数`mapStateToProps`：

```javascript
const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}
```

如果我们传递`makeMapStateToProps`给容器，容器的`connect`每个`VisibleTodosList`实例都将`mapStateToProps`通过一个私有`getVisibleTodos`选择器获得它自己的函数。无论`VisibleTodoList`容器的渲染顺序如何，Memoization现在都可以正常工作。

#### `containers/VisibleTodoList.js`

```javascript
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { makeGetVisibleTodos } from '../selectors'

const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  makeMapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

## 下一步

查看Reselect 的[官方文档](https://github.com/reactjs/reselect)以及[FAQ](https://github.com/reactjs/reselect#faq)。大多数 Redux 项目在由于派生计算过多和浪费重新渲染而导致性能问题时开始使用Reselect，所以在构建一些重要的东西之前，确保熟悉它。研究[它的源代码](https://github.com/reactjs/reselect/blob/master/src/index.js)也是有用的，所以你不会认为它是神奇的。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18