# 故障排除（Troubleshooting）

- 贡献者1人

  

这是一个分享共同问题和解决方案的地方。

这些例子使用了 React ，但是如果你使用其他的东西，你仍然应该发现它们很有用。

### 我发送一个动作时没有任何反应

有时，您正在尝试分派操作，但您的视图不会更新。为什么会这样？可能有几个原因。

#### 永远不要改变reducer参数

Redux 很容易修改传给你的`state`或`action`。请不要这样做！

Redux 假定你永远不会改变它在 reducer 中给你的对象。**每一次，你都必须返回新的状态对象。**即使你不使用像 [Immutable ](https://facebook.github.io/immutable-js/)这样的库，你也需要完全避免突变。

不变性是让 [react-redux ](https://github.com/gaearon/react-redux)有效地订阅你的状态的细粒度更新。它还具有出色的开发人员体验功能，例如使用 [redux-devtools 进行](http://github.com/gaearon/redux-devtools)时间旅行。

例如，像这样的 reducer 是错误的，因为它会改变状态：

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      // Wrong! This mutates state
      state.push({
        text: action.text,
        completed: false
      })
      return state
    case 'COMPLETE_TODO':
      // Wrong! This mutates state[action.index].
      state[action.index].completed = true
      return state
    default:
      return state
  }
}
```

它需要像这样重写：

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      // Return a new array
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      // Return a new array
      return state.map((todo, index) => {
        if (index === action.index) {
          // Copy the object before mutating
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}
```

这是更多的代码，但这正是Redux可预测和高效的原因。如果你想要更少的代码，你可以使用一个助手，像[`React.addons.update`](https://facebook.github.io/react/docs/update.html)一个简洁的语法来编写不可变的转换：

```javascript
// Before:
return state.map((todo, index) => {
  if (index === action.index) {
    return Object.assign({}, todo, {
      completed: true
    })
  }
  return todo
})

// After
return update(state, {
  [action.index]: {
    completed: {
      $set: true
    }
  }
})
```

最后，要更新对象，您需要类似于`_.extend`Underscore或更好的[`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)填充。

确保你正确使用`Object.assign`。例如，不要从你的reducer 中返回类似`Object.assign(state, newData)`的东西，而是返回`Object.assign({}, state, newData)`。这样你不会覆盖以前的`state`。

您还可以启用对象扩展运算符提议以获得更简洁的语法：

```javascript
// Before:
return state.map((todo, index) => {
  if (index === action.index) {
    return Object.assign({}, todo, {
      completed: true
    })
  }
  return todo
})

// After:
return state.map((todo, index) => {
  if (index === action.index) {
    return { ...todo, completed: true }
  }
  return todo
})
```

请注意，实验语言功能可能会发生变化。

同时留意需要深度复制的嵌套状态对象。`_.extend`和`Object.assign`做一个状态的浅拷贝。有关如何处理嵌套状态对象的建议，请参阅更新嵌套对象。

#### 别忘了调用 `dispatch(action)`

如果您定义了动作创建者，调用它将*不会*自动发送动作。例如，这段代码什么都不执行：

#### `TodoActions.js`

```javascript
export function addTodo(text) {
  return { type: 'ADD_TODO', text }
}
```

#### `AddTodo.js`

```javascript
import React, { Component } from 'react'
import { addTodo } from './TodoActions'

class AddTodo extends Component {
  handleClick() {
    // Won't work!
    addTodo('Fix the issue')
  }

  render() {
    return (
      <button onClick={() => this.handleClick()}>
        Add
      </button>
    )
  }
}
```

它不起作用，因为您的动作创建者只是一个*返回*动作的函数。实际发送它取决于你。我们无法在定义期间将您的动作创建者绑定到特定的Store实例，因为在服务器上呈现的应用程序需要为每个请求分别提供一个Redux存储。

修复方法是在商店实例上调用`dispatch()`方法：

```javascript
handleClick() {
  // Works! (but you need to grab store somehow)
  store.dispatch(addTodo('Fix the issue'))
}
```

如果您位于组件层次结构的深处，则手动传递存储会很麻烦。这就是为什么 [react-redux ](https://github.com/gaearon/react-redux)可以让你使用`connect` [高阶组件](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)，除了订阅你的 Redux 商店之外，还可以注入`dispatch`组件的道具。

固定代码如下所示：

#### `AddTodo.js`

```javascript
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { addTodo } from './TodoActions'

class AddTodo extends Component {
  handleClick() {
    // Works!
    this.props.dispatch(addTodo('Fix the issue'))
  }

  render() {
    return (
      <button onClick={() => this.handleClick()}>
        Add
      </button>
    )
  }
}

// In addition to the state, `connect` puts `dispatch` in our props.
export default connect()(AddTodo)
```

如果需要，可以手动传递`dispatch`给其他组件。

#### 确保mapStateToProps正确

你可能正确地调度行动并应用你的减速器，但是相应的状态没有被正确地转换成 props 。

## 别的东西不起作用

询问 **#redux** [Reactiflux](http://reactiflux.com/) Discord 频道，或者[创建一个问题](https://github.com/reactjs/redux/issues)。

如果你想出来了，[编辑这个文件](https://github.com/reactjs/redux/edit/master/docs/Troubleshooting.md)给下一个有同样问题的人。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18