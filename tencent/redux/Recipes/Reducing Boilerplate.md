# 减少Boilerplate (Reducing Boilerplate )

- 贡献者1人

  

Redux部分受到Flux的启发，关于Flux最常见的解释是它如何让你编写大量样板文件。在这个教程中，我们将考虑Redux如何让我们选择我们的代码是多么冗长，取决于个人风格，团队偏好，更长期的可维护性等等。

## 操作

动作是描述应用程序中发生的事情的简单对象，并且可以作为描述数据变异意图的唯一方式。重要的是，**你需要发送的对象不是样板，而是** **Redux 的基本设计选择** **之一**。

有些框架声称与 Flux 相似，但没有动作对象的概念。就可预测性而言，这是 Flux 或 Redux 的倒退。如果没有可序列化的普通对象操作，则不可能记录和重放用户会话，也不可能实现[随时间旅行的热重载](https://www.youtube.com/watch?v=xsSnOQynTHs)。如果您想直接修改数据，则不需要 Redux。

操作如下所示：

```javascript
{ type: 'ADD_TODO', text: 'Use Redux' }
{ type: 'REMOVE_TODO', id: 42 }
{ type: 'LOAD_ARTICLE', response: { ... } }
```

这是一个常见的惯例，行动有一个不断的类型，帮助 reduce （或通量中的商店）识别它们。我们建议您使用字符串而不使用[符号](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol)作为动作类型，因为字符串是可序列化的，并且通过使用符号，您可以进行录制和重放，比需要的更难。

在 Flux 中，传统上认为你会将每个动作类型定义为一个字符串常量：

```javascript
const ADD_TODO = 'ADD_TODO'
const REMOVE_TODO = 'REMOVE_TODO'
const LOAD_ARTICLE = 'LOAD_ARTICLE'
```

为什么这是有益的？**常常声称常量是不必要的，对于小型项目来说，这可能是正确的。**对于较大的项目，将动作类型定义为常量有一些好处：

- 它有助于保持命名一致，因为所有动作类型都集中在一个地方。

- 有时，您希望在处理新功能之前查看所有现有的操作。这可能是你需要的行动已经被团队中的某个人添加了，但你不知道。

- 在合并请求中添加，删除和更改的操作类型列表可帮助团队中的每个人跟踪新功能的范围和实施情况。

- 如果你在导入一个动作常量时犯了一个错误，你会得到`undefined`。Redux 在派遣此类行动时将立即抛出，并且您会很快发现错误。

您可以选择项目的约定。您可以先使用内联字符串开始，然后再转换为常量，然后再将它们分组到一个文件中。Redux 在这里没有任何意见，所以请使用您的最佳判断。

## 行动创建者

另一个常见的规定是，不是在派发动作的地方创建内联行为对象，而是创建生成它们的函数。

例如，不是用`dispatch`对象对文字调用：

```javascript
// somewhere in an event handler
dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
})
```

您可以在单独的文件中编写一个操作创建器，并将其导入到组件中：

#### `actionCreators.js`

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
```

#### `AddTodo.js`

```javascript
import { addTodo } from './actionCreators'

// somewhere in an event handler
dispatch(addTodo('Use Redux'))
```

动作创作者经常被批评为 boilerplate 。那么，你不必写他们！**如果你觉得这个更适合你的项目，你可以使用对象文字。**然而，对于你应该知道的写作动作创作者来说，有一些好处。

假设一位开发者在回顾了我们的原型之后回过头来看我们，并告诉我们我们需要最多允许三个待办事项。我们可以通过将我们的动作创建者重写为一个回调形式并使用 [redux-thunk ](https://github.com/gaearon/redux-thunk)中间件并添加一个提前退出来强制执行此操作：

```javascript
function addTodoWithoutCheck(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

export function addTodo(text) {
  // This form is allowed by Redux Thunk middleware
  // described below in “Async Action Creators” section.
  return function (dispatch, getState) {
    if (getState().todos.length === 3) {
      // Exit early
      return
    }
    dispatch(addTodoWithoutCheck(text))
  }
}
```

我们只是修改了`addTodo`动作创建者的行为，对调用代码完全不可见。**我们不必担心每个待添加待办事项的地方，以确保他们有这项检查。**动作创建者可以让你分离出派发动作的附加逻辑，从发出这些动作的实际组件中分离出来。当应用程序处于繁重的开发阶段时，它非常方便，并且需求经常变化。

### 生成动作创作者

一些框架如 [Flummox ](https://github.com/acdlite/flummox)从动作创建者函数定义中自动生成动作类型常量。这个想法是，你不需要定义`ADD_TODO`常量和`addTodo()`动作的创造者。在这种情况下，这些解决方案仍然会生成动作类型常量，但它们是隐式创建的，所以它是一种间接的级别，可能会导致混淆。我们建议显式创建您的动作类型常量。

编写简单的动作创建器可能令人厌烦，并且通常最终会产生冗余的样板代码：

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

export function editTodo(id, text) {
  return {
    type: 'EDIT_TODO',
    id,
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

您始终可以编写一个生成动作创建者的函数：

```javascript
function makeActionCreator(type, ...argNames) {
  return function (...args) {
    let action = { type }
    argNames.forEach((arg, index) => {
      action[argNames[index]] = args[index]
    })
    return action
  }
}

const ADD_TODO = 'ADD_TODO'
const EDIT_TODO = 'EDIT_TODO'
const REMOVE_TODO = 'REMOVE_TODO'

export const addTodo = makeActionCreator(ADD_TODO, 'text')
export const editTodo = makeActionCreator(EDIT_TODO, 'id', 'text')
export const removeTodo = makeActionCreator(REMOVE_TODO, 'id')
```

还有一些实用程序库可以帮助生成动作创建者，例如 [redux-act ](https://github.com/pauldijou/redux-act)和 [redux-actions ](https://github.com/acdlite/redux-actions)。这些可以帮助减少样板代码并加强对 [Flux Standard Action（FSA） ](https://github.com/acdlite/flux-standard-action)等[标准的](https://github.com/acdlite/flux-standard-action)遵守。

## 异步动作创作者

中间件允许您注入定制逻辑，在调度之前解释每个操作对象。异步操作是中间件最常见的用例。

没有任何中间件，`dispatch`只接受一个普通的对象，所以我们必须在我们的组件中执行AJAX调用：

#### `actionCreators.js`

```javascript
export function loadPostsSuccess(userId, response) {
  return {
    type: 'LOAD_POSTS_SUCCESS',
    userId,
    response
  }
}

export function loadPostsFailure(userId, error) {
  return {
    type: 'LOAD_POSTS_FAILURE',
    userId,
    error
  }
}

export function loadPostsRequest(userId) {
  return {
    type: 'LOAD_POSTS_REQUEST',
    userId
  }
}
```

#### `UserInfo.js`

```javascript
import { Component } from 'react'
import { connect } from 'react-redux'
import {
  loadPostsRequest,
  loadPostsSuccess,
  loadPostsFailure
} from './actionCreators'

class Posts extends Component {
  loadData(userId) {
    // Injected into props by React Redux `connect()` call:
    let { dispatch, posts } = this.props

    if (posts[userId]) {
      // There is cached data! Don't do anything.
      return
    }

    // Reducer can react to this action by setting
    // `isFetching` and thus letting us show a spinner.
    dispatch(loadPostsRequest(userId))

    // Reducer can react to these actions by filling the `users`.
    fetch(`http://myapi.com/users/${userId}/posts`).then(
      response => dispatch(loadPostsSuccess(userId, response)),
      error => dispatch(loadPostsFailure(userId, error))
    )
  }

  componentDidMount() {
    this.loadData(this.props.userId)
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.userId !== this.props.userId) {
      this.loadData(nextProps.userId)
    }
  }

  render() {
    if (this.props.isFetching) {
      return <p>Loading...</p>
    }

    let posts = this.props.posts.map(post =>
      <Post post={post} key={post.id} />
    )

    return <div>{posts}</div>
  }
}

export default connect(state => ({
  posts: state.posts
}))(Posts)
```

但是，由于不同的组件从相同的API端点请求数据，因此这会很快重复。此外，我们希望从许多组件中重用某些逻辑（例如，在存在缓存数据时提前退出）。

**中间件让我们可以编写更具表现力的潜在异步动作创作者。**它让我们派发除普通对象以外的东西，并解释值。例如，中间件可以“捕获”派遣的 Promise ，并将它们变成一对请求和成功/失败行为。

中间件的最简单的例子就是[简化](https://github.com/gaearon/redux-thunk)。**“Thunk” 中间件允许您将动作创建者编写为 “thunk” ，即函数返回函数。**这反过来控制：你会得到`dispatch`一个参数，所以你可以写一个动作创建器多次调度。

> 注意
>
> Thunk中间件只是中间件的一个例子。中间件不是关于“调度功能”。这是关于让你发送你使用的特定中间件知道如何处理的任何东西。Thunk中间件在您分派函数时会添加特定的行为，但它实际上取决于您使用的中间件。

考虑上面用 [redux-thunk ](https://github.com/gaearon/redux-thunk)重写的代码：

#### `actionCreators.js`

```javascript
export function loadPosts(userId) {
  // Interpreted by the thunk middleware:
  return function (dispatch, getState) {
    let { posts } = getState()
    if (posts[userId]) {
      // There is cached data! Don't do anything.
      return
    }

    dispatch({
      type: 'LOAD_POSTS_REQUEST',
      userId
    })

    // Dispatch vanilla actions asynchronously
    fetch(`http://myapi.com/users/${userId}/posts`).then(
      response =>
        dispatch({
          type: 'LOAD_POSTS_SUCCESS',
          userId,
          response
        }),
      error =>
        dispatch({
          type: 'LOAD_POSTS_FAILURE',
          userId,
          error
        })
    )
  }
}
```

#### `UserInfo.js`

```javascript
import { Component } from 'react'
import { connect } from 'react-redux'
import { loadPosts } from './actionCreators'

class Posts extends Component {
  componentDidMount() {
    this.props.dispatch(loadPosts(this.props.userId))
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.userId !== this.props.userId) {
      this.props.dispatch(loadPosts(nextProps.userId))
    }
  }

  render() {
    if (this.props.isFetching) {
      return <p>Loading...</p>
    }

    let posts = this.props.posts.map(post =>
      <Post post={post} key={post.id} />
    )

    return <div>{posts}</div>
  }
}

export default connect(state => ({
  posts: state.posts
}))(Posts)
```

这是更少的代码！如果你愿意，你仍然可以拥有“vanilla”动作创建者，比如`loadPostsSuccess`你将使用容器`loadPosts`动作创建者。

**最后，你可以编写你自己的中间件。**假设您想概括一下上面的模式，并描述如下的异步动作创建器：

```javascript
export function loadPosts(userId) {
  return {
    // Types of actions to emit before and after
    types: ['LOAD_POSTS_REQUEST', 'LOAD_POSTS_SUCCESS', 'LOAD_POSTS_FAILURE'],
    // Check the cache (optional):
    shouldCallAPI: state => !state.posts[userId],
    // Perform the fetching:
    callAPI: () => fetch(`http://myapi.com/users/${userId}/posts`),
    // Arguments to inject in begin/end actions
    payload: { userId }
  }
}
```

解释此类操作的中间件可能如下所示：

```javascript
function callAPIMiddleware({ dispatch, getState }) {
  return next => action => {
    const {
      types,
      callAPI,
      shouldCallAPI = () => true,
      payload = {}
    } = action

    if (!types) {
      // Normal action: pass it on
      return next(action)
    }

    if (
      !Array.isArray(types) ||
      types.length !== 3 ||
      !types.every(type => typeof type === 'string')
    ) {
      throw new Error('Expected an array of three string types.')
    }

    if (typeof callAPI !== 'function') {
      throw new Error('Expected callAPI to be a function.')
    }

    if (!shouldCallAPI(getState())) {
      return
    }

    const [requestType, successType, failureType] = types

    dispatch(
      Object.assign({}, payload, {
        type: requestType
      })
    )

    return callAPI().then(
      response =>
        dispatch(
          Object.assign({}, payload, {
            response,
            type: successType
          })
        ),
      error =>
        dispatch(
          Object.assign({}, payload, {
            error,
            type: failureType
          })
        )
    )
  }
}
```

在传递给`applyMiddleware(...middlewares)`之后，您可以用同样的方式编写所有API调用动作创建器：

```javascript
export function loadPosts(userId) {
  return {
    types: ['LOAD_POSTS_REQUEST', 'LOAD_POSTS_SUCCESS', 'LOAD_POSTS_FAILURE'],
    shouldCallAPI: state => !state.posts[userId],
    callAPI: () => fetch(`http://myapi.com/users/${userId}/posts`),
    payload: { userId }
  }
}

export function loadComments(postId) {
  return {
    types: [
      'LOAD_COMMENTS_REQUEST',
      'LOAD_COMMENTS_SUCCESS',
      'LOAD_COMMENTS_FAILURE'
    ],
    shouldCallAPI: state => !state.comments[postId],
    callAPI: () => fetch(`http://myapi.com/posts/${postId}/comments`),
    payload: { postId }
  }
}

export function addComment(postId, message) {
  return {
    types: [
      'ADD_COMMENT_REQUEST',
      'ADD_COMMENT_SUCCESS',
      'ADD_COMMENT_FAILURE'
    ],
    callAPI: () =>
      fetch(`http://myapi.com/posts/${postId}/comments`, {
        method: 'post',
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ message })
      }),
    payload: { postId, message }
  }
}
```

## **Reducers**

通过将更新逻辑描述为函数，Redux 大大减少了 Flux 商店的样板。函数比对象更简单，而且类别更简单。

考虑这个Flux商店：

```javascript
let _todos = []

const TodoStore = Object.assign({}, EventEmitter.prototype, {
  getAll() {
    return _todos
  }
})

AppDispatcher.register(function (action) {
  switch (action.type) {
    case ActionTypes.ADD_TODO:
      let text = action.text.trim()
      _todos.push(text)
      TodoStore.emitChange()
  }
})

export default TodoStore
```

使用Redux，同样的更新逻辑可以被描述为一个简化函数：

```javascript
export function todos(state = [], action) {
  switch (action.type) {
    case ActionTypes.ADD_TODO:
      let text = action.text.trim()
      return [...state, text]
    default:
      return state
  }
}
```

`switch`声明**不是**真正的样板。Flux真正的模板是概念性的：需要发布更新，需要使用调度程序注册 Store，需要 Store 作为对象（以及在需要通用应用程序时出现的复杂情况）。

不幸的是，许多人仍然选择Flux框架，基于它是否使用`switch`文档中的语句。如果你不喜欢`switch`，你可以用一个函数来解决这个问题，如下所示。

### 生成**Reducer**

让我们编写一个函数，让我们将 reducer 表示为从操作类型到处理程序的对象映射。例如，如果我们希望我们的`todos`缩减器被定义为这样：

```javascript
export const todos = createReducer([], {
  [ActionTypes.ADD_TODO](state, action) {
    let text = action.text.trim()
    return [...state, text]
  }
})
```

我们可以编写以下帮助程序来完成此操作：

```javascript
function createReducer(initialState, handlers) {
  return function reducer(state = initialState, action) {
    if (handlers.hasOwnProperty(action.type)) {
      return handlers[action.type](state, action)
    } else {
      return state
    }
  }
}
```

这并不困难，是吗？ Redux 默认不提供这样的帮助函数，因为有很多方法可以编写它。也许你想让它自动将普通的JS对象转换为不可变的对象来保存服务器状态。也许你想合并返回的状态与当前状态。“捕捉所有”处理程序可能有不同的方法。所有这些都取决于您在特定项目中为团队选择的约定。

Redux reducer API是`(state, action) => state`，但你如何创建这些reducer是由你决定的。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18