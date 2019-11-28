# applyMiddleware()

- 贡献者1人

  

中间件是推荐使用定制功能扩展 Redux 的方法。中间件可以让你打包存储中的`dispatch`方法，获得乐趣和利润。中间件的关键特征是它是可组合的。多个中间件可以组合在一起，其中每个中间件不需要知道链中前后的内容。

中间件最常见的用例是支持异步操作，没有太多的样板代码或依赖于像 [Rx ](https://github.com/Reactive-Extensions/RxJS)这样的库。除了正常操作外，它还可以让您分配异步操作。

例如，[redux-thunk ](https://github.com/gaearon/redux-thunk)允许动作创建者通过调度函数来反转控制。他们会收到`dispatch`作为参数，并可能异步调用。这些功能被称为 *thunk* 。中间件的另一个例子是 [redux-promise ](https://github.com/acdlite/redux-promise)。它允许您分派 [Promise ](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise)异步操作，并在 Promise 解析时分派正常操作。

中间件并不是根植于`createStore`中的也不是 Redux 体系结构的基本组成部分，但我们认为它足以在核心中获得支持。这样，在生态系统中有一种标准的扩展`dispatch`的方式，不同的中间件可以在表现力和效用上竞争。

#### 参数

- `...middleware`（*参数*）：符合Redux *中间件API 的函数*。每个中间件接收`Store`的`dispatch`和`getState`功能命名的参数，并返回一个函数。该函数将被赋予`next`中间件的调度方法，并且预计会返回一个`action`调用函数，用可能有不同的参数调用`next(action)`，或者在不同的时间，或者根本不调用它。链中的最后一个中间件将接收现实存储的`dispatch`方法作为`next`参数，从而结束链。所以，中间件签名是`({ getState, dispatch }) => next => action`。

#### 返回

（*功能*）应用给定中间件的存储增强器。商店增强器签名是`createStore => createStore'`，但是最简单的方式来应用它是作为最后一个`enhancer`参数传递它`createStore()`。

#### 示例：自定义记录器中间件

```javascript
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'

function logger({ getState }) {
  return next => action => {
    console.log('will dispatch', action)

    // Call the next dispatch method in the middleware chain.
    let returnValue = next(action)

    console.log('state after dispatch', getState())

    // This will likely be the action itself, unless
    // a middleware further in chain changed it.
    return returnValue
  }
}

let store = createStore(
  todos,
  ['Use Redux'],
  applyMiddleware(logger)
)

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (These lines will be logged by the middleware:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

#### 示例：使用Thunk中间件进行异步操作

```javascript
import { createStore, combineReducers, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
import * as reducers from './reducers'

let reducer = combineReducers(reducers)
// applyMiddleware supercharges createStore with middleware:
let store = createStore(reducer, applyMiddleware(thunk))

function fetchSecretSauce() {
  return fetch('https://www.google.com/search?q=secret+sauce')
}

// These are the normal action creators you have seen so far.
// The actions they return can be dispatched without any middleware.
// However, they only express “facts” and not the “async flow”.
function makeASandwich(forPerson, secretSauce) {
  return {
    type: 'MAKE_SANDWICH',
    forPerson,
    secretSauce
  }
}

function apologize(fromPerson, toPerson, error) {
  return {
    type: 'APOLOGIZE',
    fromPerson,
    toPerson,
    error
  }
}

function withdrawMoney(amount) {
  return {
    type: 'WITHDRAW',
    amount
  }
}

// Even without middleware, you can dispatch an action:
store.dispatch(withdrawMoney(100))

// But what do you do when you need to start an asynchronous action,
// such as an API call, or a router transition?

// Meet thunks.
// A thunk is a function that returns a function.
// This is a thunk.
function makeASandwichWithSecretSauce(forPerson) {

  // Invert control!
  // Return a function that accepts `dispatch` so we can dispatch later.
  // Thunk middleware knows how to turn thunk async actions into actions.
  return function (dispatch) {
    return fetchSecretSauce().then(
      sauce => dispatch(makeASandwich(forPerson, sauce)),
      error => dispatch(apologize('The Sandwich Shop', forPerson, error))
    )
  }
}

// Thunk middleware lets me dispatch thunk async actions
// as if they were actions!
store.dispatch(makeASandwichWithSecretSauce('Me'))

// It even takes care to return the thunk's return value
// from the dispatch, so I can chain Promises as long as I return them.
store.dispatch(makeASandwichWithSecretSauce('My wife')).then(() => {
  console.log('Done!')
})

// In fact I can write action creators that dispatch
// actions and async actions from other action creators,
// and I can build my control flow with Promises.
function makeSandwichesForEverybody() {
  return function (dispatch, getState) {
    if (!getState().sandwiches.isShopOpen) {

      // You don't have to return Promises, but it's a handy convention
      // so the caller can always call .then() on async dispatch result.
      return Promise.resolve()
    }

    // We can dispatch both plain object actions and other thunks,
    // which lets us compose the asynchronous actions in a single flow.
    return dispatch(makeASandwichWithSecretSauce('My Grandma'))
      .then(() =>
        Promise.all([
          dispatch(makeASandwichWithSecretSauce('Me')),
          dispatch(makeASandwichWithSecretSauce('My wife'))
        ])
      )
      .then(() => dispatch(makeASandwichWithSecretSauce('Our kids')))
      .then(() =>
        dispatch(
          getState().myMoney > 42
            ? withdrawMoney(42)
            : apologize('Me', 'The Sandwich Shop')
        )
      )
  }
}

// This is very useful for server side rendering, because I can wait
// until data is available, then synchronously render the app.

import { renderToString } from 'react-dom/server'

store
  .dispatch(makeSandwichesForEverybody())
  .then(() => response.send(renderToString(<MyApp store={store} />)))

// I can also dispatch a thunk async action from a component
// any time its props change to load the missing data.

import { connect } from 'react-redux'
import { Component } from 'react'

class SandwichShop extends Component {
  componentDidMount() {
    this.props.dispatch(makeASandwichWithSecretSauce(this.props.forPerson))
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.forPerson !== this.props.forPerson) {
      this.props.dispatch(makeASandwichWithSecretSauce(nextProps.forPerson))
    }
  }

  render() {
    return <p>{this.props.sandwiches.join('mustard')}</p>
  }
}

export default connect(state => ({
  sandwiches: state.sandwiches
}))(SandwichShop)
```

#### 提示

- 中间件只包装商店的`dispatch`功能。从技术上讲，中间件可以做的任何事情都可以通过包装每个`dispatch`调用来手动完成，但是在单一位置管理它并且在整个项目的规模上定义操作转换更容易。
- 如果除了使用其他存储增强器之外`applyMiddleware`，请确保将`applyMiddleware`它们放在组合链中，因为中间件可能是异步的。例如，它应该放在 [redux-devtools ](https://github.com/gaearon/redux-devtools)之前，否则DevTools将不会看到Promise中间件等发出的原始动作。
- 如果要有条件地应用中间件，请确保只在需要时导入：let middleware = a, b if (process.env.NODE_ENV !== 'production') { let c = require('some-debug-middleware') let d = require('another-debug-middleware') middleware = ...middleware, c, d } const store = createStore( reducer, preloadedState, applyMiddleware(...middleware) )这使得捆绑更容易工具来删除不需要的模块并减少构建的大小。
- 有没有想过`applyMiddleware`自己是什么？它应该是一个比中间件本身更强大的扩展机制。事实上，`applyMiddleware`是最强大的 Redux 扩展机制称为存储增强器的一个例子。这是非常不可能的，你会想要自己写一个商店增强器。商店增强器的另一个例子是 [redux-devtools](https://github.com/gaearon/redux-devtools)。中间件不如存储增强器强大，但它更易于编写。
- 中间件听起来要比实际复杂得多。真正理解中间件的唯一方法是看看现有的中间件是如何工作的，并尝试编写自己的中间件。函数嵌套可能会令人生畏，但大多数中间件实际上都是10行内存，嵌套和可组合性使得中间件系统功能强大。
- 要应用多个商店增强器，您可以使用`compose()`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18