# 核心概念（Core Concepts）

- 贡献者1人

  

Redux 本身非常简单。

想象一下，你的应用程序的状态被描述为一个普通的对象。例如，待办事项应用的状态可能如下所示：

```javascript
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

这个对象就像一个“模型”，除了没有 setter 。这是因为代码的不同部分不能任意改变状态，导致难以重现的错误。

要改变这个状态，你需要发送一个动作。一个动作是一个普通的 JavaScript 对象（注意我们如何不引入任何魔法？），它描述了发生的事情。以下是一些示例操作：

```javascript
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

强制将每一项变更都描述为一项行动，让我们清楚了解应用中发生的情况。如果有什么改变，我们知道它为什么改变。行动就像发生了什么事情的面包屑。最后，为了将状态和动作绑定在一起，我们编写了一个名为 reducer 的函数。因此，没有什么神奇的 - 它只是一个以状态和动作为参数的函数，并返回应用程序的下一个状态。为大型应用程序编写这样的功能将很难，因此我们编写管理部分状态的较小函数：

```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map(
        (todo, index) =>
          action.index === index
            ? { text: todo.text, completed: !todo.completed }
            : todo
      )
    default:
      return state
  }
}
```

然后我们编写另一个 reducer ，通过调用这两个 reducer 来查找相应的状态键来管理我们的应用程序的完整状态：

```javascript
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```

这基本上是 Redux 的整个想法。请注意，我们尚未使用任何 Redux API 。它提供了一些用于简化这种模式的实用程序，但主要想法是描述如何随着时间的推移更新状态以响应操作对象，并且您编写的代码中90％只是普通的 JavaScript ，没有使用 Redux 本身，API或任何魔法。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18