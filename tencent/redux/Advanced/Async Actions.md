# 异步操作（Async Actions）

- 贡献者1人

  

在基础指南中，我们构建了一个简单的待办事项应用程序。它完全同步。每次调度操作时, 状态都立即更新。

在本指南中，我们将构建一个不同的异步应用程序。它将使用 Reddit API 来显示选定的 subreddit 的当前标题。异步性如何适应 Redux 流程？

## 操作

当你调用一个异步 API 时，有两个关键时刻：你开始调用的那一刻，以及你收到答案（或超时）的那一刻。

这两个时刻中的每一个通常都需要改变应用程序状态; 要做到这一点，您需要调度将由减速器同步处理的正常操作。通常，对于任何 API 请求，您都希望分派至少三种不同的操作：

- **通知减速器该请求已开始的操作。** 减速器可以通过切换`isFetching`状态中的标志来处理这个动作。这样 UI 就知道是时候展示一个微调。
- **通知减速器该操作成功完成的操作。** 减法器可以通过将新数据合并到他们管理和重置`isFetching`的状态来处理此操作。用户界面会隐藏微调器，并显示提取的数据。
- **通知减速器请求失败的操作。** 减速器可以通过重置`isFetching`来处理这个动作。此外，一些减速器可能需要存储错误消息，以便 UI 可以显示它。

您可以在您的操作中使用专用`status`字段：

```javascript
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }
```

或者你可以为它们定义不同的类型：

```javascript
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

选择是否使用具有标志或多种操作类型的单一操作类型取决于您。这是一个你需要与团队决定的大会。多种类型可以减少出错的空间，但如果使用像 [redux-actions ](https://github.com/acdlite/redux-actions)这样的辅助库生成动作创建者和缩减器，这不是问题。

无论您选择哪种规定，都要坚持贯穿整个应用程序。

我们将在本教程中使用不同的类型。

## 同步动作创作者

我们首先定义我们示例应用中需要的几个同步动作类型和动作创建器。在这里，用户可以选择一个 subreddit 来显示：

#### `actions.js`

```javascript
export const SELECT_SUBREDDIT = 'SELECT_SUBREDDIT'

export function selectSubreddit(subreddit) {
  return {
    type: SELECT_SUBREDDIT,
    subreddit
  }
}
```

也可以按下“refresh”按钮来更新它：

```javascript
export const INVALIDATE_SUBREDDIT = 'INVALIDATE_SUBREDDIT'

export function invalidateSubreddit(subreddit) {
  return {
    type: INVALIDATE_SUBREDDIT,
    subreddit
  }
}
```

这些是由用户交互操纵的行为。我们还将采取另一种行动，受网络请求的支配。稍后我们将看到如何发送，但现在我们只想定义它们。

当需要获取某些 subreddit 的帖子时，我们将发送一个`REQUEST_POSTS`动作：

```javascript
export const REQUEST_POSTS = 'REQUEST_POSTS'

function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}
```

将其与`SELECT_SUBREDDIT`或`INVALIDATE_SUBREDDIT`分开是重要的。虽然它们可能会一个接一个地出现，但随着应用程序变得更加复杂，您可能希望独立于用户操作获取一些数据（例如，预取最常用的子集或偶尔刷新陈旧的数据）。您也可能想要响应路由更改，因此在早期将提取耦合到某个特定的 UI 事件并不明智。

最后，当网络请求通过时，我们将发送`RECEIVE_POSTS`：

```javascript
export const RECEIVE_POSTS = 'RECEIVE_POSTS'

function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}
```

这是我们现在需要知道的。将这些操作与网络请求一起分发的特定机制将在稍后讨论。

> 关于错误处理的注意事项在真实应用程序中，您还希望在请求失败时发出操作。我们不会在本教程中实现错误处理，但现实世界的例子显示了一种可能的方法。

## 设计状态形状

就像在基本教程中一样，在进入实现之前，您需要设计应用程序状态的形状。使用异步代码，需要处理更多的状态，所以我们需要思考。

这部分经常让初学者感到困惑，因为不清楚哪些信息描述了异步应用程序的状态，以及如何在一棵树中将其组织。

我们将从最常见的用例开始：列表。Web 应用程序经常显示事物的列表。例如，帖子列表或朋友列表。你需要弄清楚你的应用可以显示什么样的列表。您希望将它们分开存储在状态中，因为这样可以缓存它们，并且只在需要时才再次获取。

以下是我们的“Reddit 头条”应用的状态：

```javascript
{
  selectedSubreddit: 'frontend',
  postsBySubreddit: {
    frontend: {
      isFetching: true,
      didInvalidate: false,
      items: []
    },
    reactjs: {
      isFetching: false,
      didInvalidate: false,
      lastUpdated: 1439478405547,
      items: [
        {
          id: 42,
          title: 'Confusion about Flux and Relay'
        },
        {
          id: 500,
          title: 'Creating a Simple Application Using React JS and Flux Architecture'
        }
      ]
    }
  }
}
```

这里有几个重要的部分：

- 我们分别存储每个 subreddit 的信息，所以我们可以缓存每个 subreddit。当用户第二次在它们之间切换时，更新将是即时的，除非我们想要，否则我们不需要重新提取。不要担心所有这些项目都在内存中：除非您处理数以万计的项目，并且用户很少关闭该选项卡，否则不需要任何清理。
- 对于每个项目列表，您都需要存储`isFetching`以显示一个微调器，`didInvalidate`以便稍后在数据过期时切换它，`lastUpdated`以便知道它最后一次何时提取以及`items`它们自己。在实际的应用程序，你还需要像`fetchedPageCount`和`nextPageUrl`存储分页状态。

> 关于嵌套实体的注意事项在本例中，我们将收到的项目与分页信息一起存储。但是，如果您的嵌套实体互相引用，或者让用户编辑项目，则此方法将无法正常工作。想象一下，用户想要编辑一个提取的帖子，但是这个帖子在状态树的几个地方被复制。这实施起来非常痛苦。如果你有嵌套的实体，或者如果你让用户编辑收到的实体，你应该把它们分开保存在状态中，就好像它是一个数据库一样。在分页信息中，您只能通过它们的ID来引用它们。这可以让你始终保持最新状态。真实世界的例子与[normalizr](https://github.com/paularmstrong/normalizr)一起显示了这种方法规范化嵌套的API响应。使用这种方法，您的状态可能如下所示：{ selectedSubreddit: 'frontend', entities: { users: { 2: { id: 2, name: 'Andrew' } }, posts: { 42: { id: 42, title: 'Confusion about Flux and Relay', author: 2 }, 100: { id: 100, title: 'Creating a Simple Application Using React JS and Flux Architecture', author: 2 } } }, postsBySubreddit: { frontend: { isFetching: true, didInvalidate: false, items: [] }, reactjs: { isFetching: false, didInvalidate: false, lastUpdated: 1439478405547, items: 42, 100 } } }

在本指南中，我们不会正常化的实体，但这是你为了更动态的应用程序而应该考虑的事情。

## 处理操作

在讨论与网络请求一起的调度操作细节之前，我们将为我们上面定义的操作编写减速器。

> 减速机组成的注意事项在此，我们假设您了解减速机的组成`combineReducers()`，如基本指南中的分裂减速机部分所述。如果你不了解，请先阅读它。

#### `reducers.js`

```javascript
import { combineReducers } from 'redux'
import {
  SELECT_SUBREDDIT,
  INVALIDATE_SUBREDDIT,
  REQUEST_POSTS,
  RECEIVE_POSTS
} from '../actions'

function selectedSubreddit(state = 'reactjs', action) {
  switch (action.type) {
    case SELECT_SUBREDDIT:
      return action.subreddit
    default:
      return state
  }
}

function posts(
  state = {
    isFetching: false,
    didInvalidate: false,
    items: []
  },
  action
) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
      return Object.assign({}, state, {
        didInvalidate: true
      })
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        isFetching: true,
        didInvalidate: false
      })
    case RECEIVE_POSTS:
      return Object.assign({}, state, {
        isFetching: false,
        didInvalidate: false,
        items: action.posts,
        lastUpdated: action.receivedAt
      })
    default:
      return state
  }
}

function postsBySubreddit(state = {}, action) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
    case RECEIVE_POSTS:
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        [action.subreddit]: posts(state[action.subreddit], action)
      })
    default:
      return state
  }
}

const rootReducer = combineReducers({
  postsBySubreddit,
  selectedSubreddit
})

export default rootReducer
```

在这个代码中，有两个有趣的部分：

- 我们使用 ES6 计算的属性语法，所以我们可以`state[action.subreddit]`用`Object.assign()`简洁的方式进行更新。This: return Object.assign({}, state, { action.subreddit: posts(stateaction.subreddit, action) }) is equivalent to this: let nextState = {} nextStateaction.subreddit = posts(stateaction.subreddit, action) return Object.assign({}, state, nextState)
- 我们提取的`posts(state, action)`是管理特定帖子列表的状态。这只是减速机的组成！如何将 reducer 分解为更小的 reducer 是我们的选择，在这种情况下，我们将对象内的更新项目委托给`posts`减速器。现实世界的例子更进一步，展示了如何为参数化分页减速器创建一个减速器工厂。

请记住，减速器只是功能，所以您可以尽可能多地使用功能组合和高阶功能。

## 异步动作创作者

最后，我们如何使用我们之前定义的同步动作创建器和网络请求？使用 Redux 的标准方法是使用 [Redux Thunk 中间件](https://github.com/gaearon/redux-thunk)。它包含在一个称作`redux-thunk`单独的的包中。我们将在后面解释中间件如何工作。现在，只需要知道一件重要的事情：通过使用这个特定的中间件，动作创建者可以返回一个函数而不是一个动作对象。这样，动作创建者就变成了一个 [thunk ](https://en.wikipedia.org/wiki/Thunk)。

当动作创建者返回一个函数时，该函数将由 Redux Thunk 中间件执行。这个函数不需要是纯粹的；它因此被允许有副作用，包括执行异步 API 调用。该函数还可以调度动作，就像我们之前定义的那些同步动作。

我们仍然可以在我们的`actions.js`文件中定义这些特殊的 thunk 动作创建者：

#### `actions.js`

```javascript
import fetch from 'isomorphic-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

// Meet our first thunk action creator!
// Though its insides are different, you would use it just like any other action creator:
// store.dispatch(fetchPosts('reactjs'))

export function fetchPosts(subreddit) {
  // Thunk middleware knows how to handle functions.
  // It passes the dispatch method as an argument to the function,
  // thus making it able to dispatch actions itself.

  return function (dispatch) {
    // First dispatch: the app state is updated to inform
    // that the API call is starting.

    dispatch(requestPosts(subreddit))

    // The function called by the thunk middleware can return a value,
    // that is passed on as the return value of the dispatch method.

    // In this case, we return a promise to wait for.
    // This is not required by thunk middleware, but it is convenient for us.

    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then(
        response => response.json(),
        // Do not use catch, because that will also catch
        // any errors in the dispatch and resulting render,
        // causing an loop of 'Unexpected batch number' errors.
        // https://github.com/facebook/react/issues/6895
        error => console.log('An error occured.', error)
      )
      .then(json =>
        // We can dispatch many times!
        // Here, we update the app state with the results of the API call.

        dispatch(receivePosts(subreddit, json))
      )
  }
}
```

> `fetch`注意事项 我们在示例中使用了[`fetch`API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)。它是一种新的 API，可用于取代`XMLHttpRequest`大多数常见需求的网络请求。因为大多数浏览器本身还不支持，所以我们建议您使用[`isomorphic-fetch`](https://github.com/matthew-andrews/isomorphic-fetch)library://在您使用`fetch`从 'isomorphic-fetch' 进行导入获取的每个文件中执行此操作在内部，它在客户端使用[`whatwg-fetch`polyfill](https://github.com/github/fetch)和服务器上使用[`node-fetch`](https://github.com/bitinn/node-fetch)，因此如果您将应用更改为 [universal，](https://medium.com/@mjackson/universal-javascript-4761051b7ae9)则无需更改 API 调用。请注意，任何`fetch`polyfill 都假定一个 [Promise ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)polyfill 已经存在。最简单的方法来确保你有一个 Promise polyfill，它是在任何其他代码运行之前在你的入口点启用 Babel 的 ES6 polyfill：
>
> // 在应用程序中的任何其他代码导入 'babel-polyfill' 之前执行一次操作

我们如何在调度机制中包含 Redux Thunk 中间件？我们从 Redux 中使用`applyMiddleware()` 的存储增强器，如下所示：

#### `index.js`

```javascript
import thunkMiddleware from 'redux-thunk'
import { createLogger } from 'redux-logger'
import { createStore, applyMiddleware } from 'redux'
import { selectSubreddit, fetchPosts } from './actions'
import rootReducer from './reducers'

const loggerMiddleware = createLogger()

const store = createStore(
  rootReducer,
  applyMiddleware(
    thunkMiddleware, // lets us dispatch() functions
    loggerMiddleware // neat middleware that logs actions
  )
)

store.dispatch(selectSubreddit('reactjs'))
store
  .dispatch(fetchPosts('reactjs'))
  .then(() => console.log(store.getState()))
```

关于 thunk 的好处是他们可以调度彼此的结果：

#### `actions.js`

```javascript
import fetch from 'isomorphic-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

function fetchPosts(subreddit) {
  return dispatch => {
    dispatch(requestPosts(subreddit))
    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then(response => response.json())
      .then(json => dispatch(receivePosts(subreddit, json)))
  }
}

function shouldFetchPosts(state, subreddit) {
  const posts = state.postsBySubreddit[subreddit]
  if (!posts) {
    return true
  } else if (posts.isFetching) {
    return false
  } else {
    return posts.didInvalidate
  }
}

export function fetchPostsIfNeeded(subreddit) {
  // Note that the function also receives getState()
  // which lets you choose what to dispatch next.

  // This is useful for avoiding a network request if
  // a cached value is already available.

  return (dispatch, getState) => {
    if (shouldFetchPosts(getState(), subreddit)) {
      // Dispatch a thunk from thunk!
      return dispatch(fetchPosts(subreddit))
    } else {
      // Let the calling code know there's nothing to wait for.
      return Promise.resolve()
    }
  }
}
```

这让我们可以逐渐编写更复杂的异步控制流程，而使用的代码可以保持几乎相同：

#### `index.js`

```javascript
store
  .dispatch(fetchPostsIfNeeded('reactjs'))
  .then(() => console.log(store.getState()))
```

> 关于服务器渲染的注意事项
>
> 异步动作创建者对于服务器渲染特别方便。您可以创建存储，调度单个异步操作创建器，以调度其他异步操作创建者为整个应用程序部分提取数据，并且仅在它返回的 Promise 完成后才呈现。然后，您的存储将已经在渲染之前满足您所需的状态。

[Thunk 中间件](https://github.com/gaearon/redux-thunk)并不是在Redux中编排异步操作的唯一方法：

- 您可以使用 [redux-promise ](https://github.com/acdlite/redux-promise)或 [redux-promise-middleware ](https://github.com/pburtchaell/redux-promise-middleware)来分派 Promises 而不是函数。
- 您可以使用 [redux-observable ](https://github.com/redux-observable/redux-observable)来分派 Observables。
- 您可以使用 [redux-saga ](https://github.com/yelouafi/redux-saga/)中间件来构建更复杂的异步操作。
- 您可以使用 [redux-pack ](https://github.com/lelandrichardson/redux-pack)中间件来分派基于承诺的异步操作。
- 您甚至可以编写自定义中间件来描述对API的调用，就像现实世界中的示例一样。

您可以尝试几个选项，选择您喜欢的规定，并遵循它，不管是否使用中间件。

## 连接到用户界面

调度异步操作与调度同步操作没有什么不同，所以我们不会详细讨论这一点。有关从 React 组件对使用 Redux 的介绍，请参阅用法与 React。请参阅示例：Reddit API 以获取本示例中讨论的完整源代码。

## 下一步

阅读异步流程以回顾异步操作如何适应Redux流程。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18