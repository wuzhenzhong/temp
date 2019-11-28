# 中间件（Middleware）

- 贡献者4人

  

您已经在异步操作示例中看到了中间件的操作。如果您已经使用了像 [Express ](http://expressjs.com/)和 [Koa ](http://koajs.com/)等服务器端库，那么您也可能已经熟悉了*中间件*的概念。在这些框架中，中间件是可以在接收请求的框架和生成响应的框架之间放置的一些代码。例如，Express 或 Koa 中间件可能会添加 CORS 标头，日志记录，压缩等等。中间件的最大特点是它可以组合在一个链中。您可以在一个项目中使用多个独立的第三方中间件。

Redux 中间件解决了与 Express 或 Koa 中间件不同的问题，但概念上类似。**它提供了一个第三方扩展点，用于分派一个动作，以及它到达reducer的那一刻。**人们使用 Redux 中间件进行日志记录，崩溃报告，与异步 API 对话，路由等等。

本文分为深入介绍以帮助您理解概念，以及一些实际示例，以最终展示中间件的强大功能。您可能会发现在它们之间来回切换是有帮助的，因为您在无聊和灵感之间翻转。

## 了解中间件

尽管中间件可用于各种各样的事情，包括异步 API 调用，但了解其来源非常重要。我们将通过使用日志记录和崩溃报告作为示例，引导您完成导致中间件的思考过程。

### 问题：记录

Redux 的好处之一是它使状态变化可预测和透明。每次发送操作时，都会计算并保存新状态。国家不能自行改变，它只能因具体行动而改变。

如果我们记录了应用程序中发生的每个操作以及之后计算的状态，这不是很好吗？当出现问题时，我们可以回顾我们的日志，并找出哪一个行为影响了状态。

![img](https://ask.qcloudimg.com/http-save/devdocs/qmxwz4vkva.png)

如何用 Redux 处理这个问题？

### Attempt #1: Logging Manually

最基本的解决方案就是每次调用`store.dispatch(action)`时只记录一次动作和下一个状态。这不是一个真正的解决方案，而只是理解问题的第一步。

> 注意
>
> 如果您使用 [react-redux ](https://github.com/gaearon/react-redux)或类似的绑定，您可能无法直接访问组件中的存储实例。在接下来的几段中，只是假设你明确地将存储关闭。

假设你在创建待办事项时调用了这个：

```javascript
store.dispatch(addTodo('Use Redux'))
```

要记录动作和状态，可以将其更改为如下所示：

```javascript
let action = addTodo('Use Redux')

console.log('dispatching', action)
store.dispatch(action)
console.log('next state', store.getState())
```

这会产生所需的效果，但您不希望每次都这样做。

### Attempt #2: Wrapping Dispatch

你可以提取日志到一个函数中：

```javascript
function dispatchAndLog(store, action) {
  console.log('dispatching', action)
  store.dispatch(action)
  console.log('next state', store.getState())
}
```

您可以随处使用它，而不是`store.dispatch()`：

```javascript
dispatchAndLog(store, addTodo('Use Redux'))
```

我们可以在这里结束，但每次都导入一个特殊的函数并不是很方便。

### Attempt #3: Monkeypatching Dispatch

如果我们只是替换`dispatch存储`实例上的功能呢？Redux 存储只是一个简单的对象，只有几个方法，而我们正在编写JavaScript，所以我们可以简单地实现这个`dispatch`实现：

```javascript
let next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}
```

这已经接近我们想要的了！无论我们在哪里发送动作，它都会保证被记录。Monkeypatching 从来没有感觉正确，但我们现在可以忍受这一点。

### 问题：崩溃报告

如果我们想要应用**多个**这样的转换到`dispatch`呢？

我想到的另一个有用的转换是报告生产中的 JavaScript 错误。全局`window.onerror`事件不可靠，因为它不提供某些较旧浏览器的堆栈信息，这对了解错误发生的原因至关重要。

如果由于调度某个动作而导致错误发生，我们会将它发送给崩溃报告服务，如 [Sentry ](https://getsentry.com/welcome/)的堆栈跟踪，导致错误的操作以及当前状态？这样在开发过程中重现错误就容易多了。

无论怎样，我们必须将日志记录和崩溃报告分开。理想情况下，我们希望他们成为不同的模块，可能在不同的包中。否则，我们不能拥有这样的实用程序的生态系统。（提示：我们正在慢慢接触什么是中间件！）

如果日志记录和崩溃报告是单独的实用程序，则它们可能如下所示：

```javascript
function patchStoreToAddLogging(store) {
  let next = store.dispatch
  store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}

function patchStoreToAddCrashReporting(store) {
  let next = store.dispatch
  store.dispatch = function dispatchAndReportErrors(action) {
    try {
      return next(action)
    } catch (err) {
      console.error('Caught an exception!', err)
      Raven.captureException(err, {
        extra: {
          action,
          state: store.getState()
        }
      })
      throw err
    }
  }
}
```

如果这些函数作为单独的模块发布，我们可以稍后使用它们来修补我们的存储：

```javascript
patchStoreToAddLogging(store)
patchStoreToAddCrashReporting(store)
```

尽管如此，这并不好。

### Attempt #4: Hiding Monkeypatching

Monkeypatching 是一个破解。“替换你喜欢的任何方法”，这是什么样的 API？让我们来弄清楚它的本质。以前，我们的职能被取代`store.dispatch`。如果他们*返回*新的`dispatch`功能呢？

```javascript
function logger(store) {
  let next = store.dispatch

  // Previously:
  // store.dispatch = function dispatchAndLog(action) {

  return function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}
```

我们可以在 Redux 中提供一个帮助器，将实际的 monkeypatching 作为实现细节应用：

```javascript
function applyMiddlewareByMonkeypatching(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  // Transform dispatch function with each middleware.
  middlewares.forEach(middleware =>
    store.dispatch = middleware(store)
  )
}
```

我们可以用它来应用这样的多个中间件：

```javascript
applyMiddlewareByMonkeypatching(store, [logger, crashReporter])
```

然而，它仍然是 monkeypatching。

我们将其隐藏在库内并不会改变这一事实。

### Attempt #5: Removing Monkeypatching

为什么我们甚至覆盖`dispatch`？当然，为了能够稍后调用它，但还有另一个原因：为了使每个中间件都可以访问（并调用）之前包装的`store.dispatch`：

```javascript
function logger(store) {
  // Must point to the function returned by the previous middleware:
  let next = store.dispatch

  return function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}
```

链接中间件至关重要！

如果在处理完第一个中间件后`applyMiddlewareByMonkeypatching`不立即分配`store.dispatch`，`store.dispatch`则会一直指向原来的`dispatch`函数。那么第二个中间件也将被绑定到原始`dispatch`函数。

但是还有一种不同的方式来启用链接。中间件可以接受`next()`调度函数作为参数，而不是从`store`实例读取它。

```javascript
function logger(store) {
  return function wrapDispatchToAddLogging(next) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action)
      let result = next(action)
      console.log('next state', store.getState())
      return result
    }
  }
}
```

这是一个[“我们需要更深入”](http://knowyourmeme.com/memes/we-need-to-go-deeper)的时刻，所以这可能需要一段时间才有意义。函数级联感觉吓人。ES6箭头功能使眼睛更容易[卷曲](https://en.wikipedia.org/wiki/Currying)：

```javascript
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}
```

**这正是 Redux 中间件的样子。**

现在，中间件采用`next()`调度函数，并返回一个调度功能，该调度功能反过来用作`next()`左侧的中间件，依此类推。访问某些像`getState()`的存储方法仍然很有用，因此可以将`store`保持作为顶级参数。

### Attempt #6: Naïvely Applying the Middleware

为了替换`applyMiddlewareByMonkeypatching()`，我们可以写出`applyMiddleware()`第一个获得最终的完全包装`dispatch()`函数，并使用它返回存储的副本：

```javascript
// Warning: Naïve implementation!
// That's *not* Redux API.
function applyMiddleware(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()
  let dispatch = store.dispatch
  middlewares.forEach(middleware =>
    dispatch = middleware(store)(dispatch)
  )
  return Object.assign({}, store, { dispatch })
}
```

用 Redux 实施`applyMiddleware()`的传输是相似的，但**在三个重要方面有所不同**：

- 它只将存储 API 的一部分公开给中间件：`dispatch(action)`和`getState()`。

- 请确保如果您调用从中间件中`store.dispatch(action)`而不是从中间件调用`next(action)`，该操作实际上会遍历整个中间件链，包括当前的中间件，这确实会带来一些诡计。正如我们以前所见，这对于异步中间件非常有用。

- 为确保您只能应用中间件一次，它可以运行`createStore()`而不是`store`自身运行。而不是`(store, middlewares) => store`，它的签名是`(...middlewares) => (createStore) => createStore`。

由于在使用`createStore()`函数之前应用该函数很麻烦，因此`createStore()`接受一个可选的最后一个参数来指定这些函数。

### 最后的方法

鉴于我们刚才写的这个中间件：

```javascript
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}
```

以下是如何将其应用于 Redux 存储的方法：

```javascript
import { createStore, combineReducers, applyMiddleware } from 'redux'

let todoApp = combineReducers(reducers)
let store = createStore(
  todoApp,
  // applyMiddleware() tells createStore() how to handle middleware
  applyMiddleware(logger, crashReporter)
)
```

现在，分派给商店实例的任何操作都将流经`logger`和`crashReporter`：

```javascript
// Will flow through both logger and crashReporter middleware!
store.dispatch(addTodo('Use Redux'))
```

## 七个例子

如果你的头脑从阅读上面的章节中解脱出来，想象一下写下它的感觉。本部分旨在为您和我放松身心，并有助于您转换注意力。

下面的每个功能都是有效的 Redux 中间件。它们不是同样有用，但至少它们同样有趣。

```javascript
/**
 * Logs all actions and states after they are dispatched.
 */
const logger = store => next => action => {
  console.group(action.type)
  console.info('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  console.groupEnd(action.type)
  return result
}

/**
 * Sends crash reports as state is updated and listeners are notified.
 */
const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}

/**
 * Schedules actions with { meta: { delay: N } } to be delayed by N milliseconds.
 * Makes `dispatch` return a function to cancel the timeout in this case.
 */
const timeoutScheduler = store => next => action => {
  if (!action.meta || !action.meta.delay) {
    return next(action)
  }

  let timeoutId = setTimeout(
    () => next(action),
    action.meta.delay
  )

  return function cancel() {
    clearTimeout(timeoutId)
  }
}

/**
 * Schedules actions with { meta: { raf: true } } to be dispatched inside a rAF loop
 * frame.  Makes `dispatch` return a function to remove the action from the queue in
 * this case.
 */
const rafScheduler = store => next => {
  let queuedActions = []
  let frame = null

  function loop() {
    frame = null
    try {
      if (queuedActions.length) {
        next(queuedActions.shift())
      }
    } finally {
      maybeRaf()
    }
  }

  function maybeRaf() {
    if (queuedActions.length && !frame) {
      frame = requestAnimationFrame(loop)
    }
  }

  return action => {
    if (!action.meta || !action.meta.raf) {
      return next(action)
    }

    queuedActions.push(action)
    maybeRaf()

    return function cancel() {
      queuedActions = queuedActions.filter(a => a !== action)
    }
  }
}

/**
 * Lets you dispatch promises in addition to actions.
 * If the promise is resolved, its result will be dispatched as an action.
 * The promise is returned from `dispatch` so the caller may handle rejection.
 */
const vanillaPromise = store => next => action => {
  if (typeof action.then !== 'function') {
    return next(action)
  }

  return Promise.resolve(action).then(store.dispatch)
}

/**
 * Lets you dispatch special actions with a { promise } field.
 *
 * This middleware will turn them into a single action at the beginning,
 * and a single success (or failure) action when the `promise` resolves.
 *
 * For convenience, `dispatch` will return the promise so the caller can wait.
 */
const readyStatePromise = store => next => action => {
  if (!action.promise) {
    return next(action)
  }

  function makeAction(ready, data) {
    let newAction = Object.assign({}, action, { ready }, data)
    delete newAction.promise
    return newAction
  }

  next(makeAction(false))
  return action.promise.then(
    result => next(makeAction(true, { result })),
    error => next(makeAction(true, { error }))
  )
}

/**
 * Lets you dispatch a function instead of an action.
 * This function will receive `dispatch` and `getState` as arguments.
 *
 * Useful for early exits (conditions over `getState()`), as well
 * as for async control flow (it can `dispatch()` something else).
 *
 * `dispatch` will return the return value of the dispatched function.
 */
const thunk = store => next => action =>
  typeof action === 'function'
    ? action(store.dispatch, store.getState)
    : next(action)

// You can use all of them! (It doesn't mean you should.)
let todoApp = combineReducers(reducers)
let store = createStore(
  todoApp,
  applyMiddleware(
    rafScheduler,
    timeoutScheduler,
    thunk,
    vanillaPromise,
    readyStatePromise,
    logger,
    crashReporter
  )
)
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-02-18