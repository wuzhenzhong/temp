# 三个原则（Three Principles）

- 贡献者1人

  

Redux可以用三个基本原则来描述：

### 单一的事实来源

**整个应用程序的状态** **存储在单个存储区中的对象树中** **。**

这使得创建通用应用程序变得很容易，因为您的服务器的状态可以被序列化并融入客户端，而无需额外的编码工作。单个状态树还使调试或检查应用程序变得更加容易; 它还使您能够持续开发您的应用程序状态，从而缩短开发周期。一些传统上难以实现的功能 - 例如撤消/重做 - 如果您的所有状态都存储在单个树中，则可能会突然变得无用。

```javascript
console.log(store.getState())

/* Prints
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
*/
```

### 状态是只读的

**改变状态的唯一方法是发出一个动作，一个描述发生事件的对象。**

确保了视图和网络回调都不会直接写入状态。相反，他们表达了转变状态的意图。因为所有的变化都是集中的，并且以严格的顺序依次发生，所以没有细微的竞争条件需要注意。由于动作只是普通的对象，因此可以对其进行记录，序列化，存储以及稍后重放以用于调试或测试目的。

```javascript
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

### 使用纯功能进行更改

**要指定状态树如何通过操作进行转换，您需要编写纯粹的** **reducer。**

Reducers只是一个纯函数，它会采用先前的状态和一个动作，并返回下一个状态。记住要返回新的状态对象，而不是改变以前的状态。您可以从一个简化器开始，随着您的应用程序的增长，将其分解成更小的简化器，以管理状态树的特定部分。因为reducer只是函数，所以您可以控制它们的调用顺序，传递其他数据，甚至为常见任务（如分页）创建可重用的reducers。

```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
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

import { combineReducers, createStore } from 'redux'
const reducer = combineReducers({ visibilityFilter, todos })
const store = createStore(reducer)
```

现在你已经知道 Redux 的全部内容了。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18