# bindActionCreators()

- 贡献者1人

  

将其值为动作创建者的对象转换为具有相同键的对象，但将每个动作创建者都包装到`dispatch`调用中，以便可以直接调用它们。

通常你应该直接调用`dispatch`你的`Store`实例。如果您将 Reux 与 React 一起使用，[react-redux ](https://github.com/gaearon/react-redux)将为您提供该`dispatch`功能，以便您可以直接调用它。

唯一的`bindActionCreators`用例是当你想将某些动作创建者传递给一个不知道Redux的组件时，并且你不想传递`dispatch`或Redux存储到它其中。

为了方便起见，您还可以传递一个函数作为第一个参数，并返回一个函数。

#### 参数

1. `actionCreators`（*函数*或*对象*）：动作创建者或其值为动作创建者的对象。
2. `dispatch`（*功能*）：在`Store`实例`dispatch`上可用的功能。

#### 返回

（*函数*或*对象*）：模仿原始对象的对象，但每个函数立即调度相应动作创建者返回的动作。如果你传递了一个函数`actionCreators`，返回值也将是一个函数。

#### 示例

#### `TodoActionCreators.js`

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

export function removeTodo(id) {
  return {
    type: 'REMOVE_TODO',
    id
  }
}
```

#### `SomeComponent.js`

```javascript
import { Component } from 'react'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import * as TodoActionCreators from './TodoActionCreators'
console.log(TodoActionCreators)
// {
//   addTodo: Function,
//   removeTodo: Function
// }

class TodoListContainer extends Component {
  componentDidMount() {
    // Injected by react-redux:
    let { dispatch } = this.props

    // Note: this won't work:
    // TodoActionCreators.addTodo('Use Redux')

    // You're just calling a function that creates an action.
    // You must dispatch the action, too!

    // This will work:
    let action = TodoActionCreators.addTodo('Use Redux')
    dispatch(action)
  }

  render() {
    // Injected by react-redux:
    let { todos, dispatch } = this.props

    // Here's a good use case for bindActionCreators:
    // You want a child component to be completely unaware of Redux.

    let boundActionCreators = bindActionCreators(TodoActionCreators, dispatch)
    console.log(boundActionCreators)
    // {
    //   addTodo: Function,
    //   removeTodo: Function
    // }

    return <TodoList todos={todos} {...boundActionCreators} />

    // An alternative to bindActionCreators is to pass
    // just the dispatch function down, but then your child component
    // needs to import action creators and know about them.

    // return <TodoList todos={todos} dispatch={dispatch} />
  }
}

export default connect(
  state => ({ todos: state.todos })
)(TodoListContainer)
```

#### 提示

- 您可能会问：为什么我们不能立即将动作创建者绑定到商店实例，就像古典 Flux 中那样？问题在于，这对于需要在服务器上呈现的通用应用程序来说效果不佳。很可能您希望每个请求都有一个单独的商店实例，以便您可以使用不同的数据进行准备，但在定义期间绑定操作创建者意味着您对所有请求都停留在单个商店实例中。
- 如果你使用 ES5，为了代替`import * as`语法，你可以传递`require('./TodoActionCreators')`给`bindActionCreators`作为第一个参数。它唯一关心的是`actionCreators`参数的值是函数。模块系统无关紧要。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18