# 基本减速器结构与样式( Basic Reducer Structure and State Shape )

- 贡献者1人

  

## 基本减速机结构

首先，重要要明白，你的整个应用程序实际上只有**一个 reducer 函数**：`createStore`作为第一个参数传入的函数。单一的reducer功能最终需要做几件事情：

- 减速器第一次被调用时，`state`值将会是`undefined`。在处理传入动作之前，reducer需要通过提供默认状态值来处理这种情况。

- 它需要查看以前的状态和派遣的行动，并确定需要完成哪些工作

- 假设需要进行实际更改，则需要使用更新后的数据创建新对象和数组，并返回这些数据

- 如果不需要更改，它应该按原样返回现有状态。

编写Reducer逻辑最简单的方法是将所有内容放入单个函数声明中，如下所示：

```javascript
function counter(state, action) {
  if (typeof state === 'undefined') {
    state = 0; // If state is undefined, initialize it with a default value
  }

  if (action.type === 'INCREMENT') {
    return state + 1;
  } 
  else if (action.type === 'DECREMENT') {
    return state - 1;
  } 
  else {
    return state; // In case an action is passed in we don't understand
  }
}
```

注意这个简单的功能满足了所有的基本要求。如果不存在，它会返回一个默认值，初始化商店; 它根据动作类型确定需要进行哪种更新并返回新值; 如果不需要完成工作，它将返回以前的状态。

这个减速器可以做一些简单的调整。首先，重复`if`/ `else`很快就会令人厌烦，所以使用`switch`非常常见。其次，我们可以使用ES6的默认参数值来处理最初的“不存在数据”情况。随着这些变化，减速器将如下所示：

```javascript
function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

这是典型的Redux减速器功能使用的基本结构。

## 基本状态形状

Redux鼓励您根据需要管理的数据考虑您的应用程序。任何时间点的数据都是应用程序的“ **状态** ”，并且该状态的结构和组织通常称为其“ **形状** ”。你的状态在你如何构建你的reducer逻辑中起着重要的作用。

Redux状态通常有一个简单的Javascript对象作为状态树的顶部。（当然也可能有其他类型的数据，例如单个数字，数组或特定的数据结构，但大多数库都假定顶层的值是一个普通的对象。）最常见的组织方式为顶级对象内的数据将进一步将数据划分为子树，其中每个顶级键代表相关数据的某个“域”或“切片”。例如，一个基本的Todo应用程序的状态可能如下所示：

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

在这个例子中，`todos`和`visibilityFilter`都是状态中的顶级密钥，并且每个密钥都代表某个特定概念的“切片”数据。

大多数应用程序处理多种类型的数据，大致可以分为三类：

- **域数据**：应用程序需要显示，使用或修改的数据（例如“从服务器检索到的所有Todos”）

- **应用程序状态**：特定于应用程序行为的数据（例如“当前选择了待办事项#5”或“请求正在处理获取待办事项”）

- **用户界面状态**：表示用户界面当前如何显示的数据（如“EditTodo模式对话框当前处于打开状态”）

由于商店代表了应用程序的核心，因此应该根据域数据和应用程序状态定义状态形状，而不是您的UI组件树。 作为一个例子，state.leftPane.todoList.todos的形状并不是一个好主意，因为“todos”的概念是整个应用程序的核心，而不仅仅是UI的一个部分。 待办事项切片应位于状态树的顶部。

将有*很少*是你的 UI 树和你的状态形状之间的1对1的对应关系。这个例外情况可能是，如果您明确地跟踪 Redux 存储中 UI 数据的各个方面，但即使如此，UI 数据的形状和域数据的形状也可能不同。

一个典型的应用程序的状态可能大致如下所示：

```javascript
{
    domainData1 : {},
    domainData2 : {},
    appState1 : {},
    appState2 : {},
    ui : {
        uiState1 : {},
        uiState2 : {},
    }
}
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18