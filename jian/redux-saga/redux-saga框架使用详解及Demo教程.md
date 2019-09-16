# redux-saga框架使用详解及Demo教程

12017.12.23 18:10:26字数 2025阅读 9556

# redux-saga框架使用详解及Demo教程

前面我们讲解过redux框架和dva框架的基本使用，因为dva框架中effects模块设计到了redux-saga中的知识点，可能有的同学们会用dva框架，但是对redux-saga又不是很熟悉，今天我们就来简单的讲解下saga框架的主要API和如何配合redux框架使用

# redux-saga 官方地址

http://leonshi.com/redux-saga-in-chinese/index.html

# Demo运行效果图

- todoList



![img](https://upload-images.jianshu.io/upload_images/6342050-2b2da14fa69e651d.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

gif

- CounterApp



![img](https://upload-images.jianshu.io/upload_images/6342050-11347c0b7eb1a946.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

gif

# 示例Demo地址

redux-saga-Demo作者还是按照以前的风格，提供了两个不同的版本，简单的 CounterApp， 稍复杂的 TodoList

- CounterApp

https://github.com/guangqiang-liu/redux-saga-counterApp

- todoList

https://github.com/guangqiang-liu/redux-saga-todoListDemo

# 什么是redux-saga

> redux-saga 是一个用于管理 Redux 应用异步操作的中间件（又称异步 action）。 redux-saga 通过创建 Sagas 将所有的异步操作逻辑收集在一个地方集中处理，可以用来代替 redux-thunk 中间件。

这意味着应用的逻辑会存在两个地方：

- Reducers 负责处理 action 的 state 更新
- Sagas 负责协调那些复杂或异步的操作

Sagas是通过Generator函数来创建的，如果有不熟悉 Generator函数使用的，[请查看阮老师对Generator的介绍](http://www.ruanyifeng.com/blog/2015/04/generator.html)

Sagas 不同于thunks，thunks 是在action被创建时调用，而 Sagas只会在应用启动时调用（但初始启动的 Sagas 可能会动态调用其他 Sagas），Sagas 可以被看作是在后台运行的进程，Sagas 监听发起的action，然后决定基于这个 action来做什么：是发起一个异步调用（比如一个 fetch 请求），还是发起其他的action到Store，甚至是调用其他的 Sagas

在 redux-saga 的世界里，所有的任务都通用 yield Effects 来完成（Effect 可以看作是 redux-saga 的任务单元）。Effects 都是简单的 Javascript 对象，包含了要被 Saga middleware 执行的信息（打个比方，你可以看到 Redux action其实是一个个包含执行信息的对象）， redux-saga 为各项任务提供了各种Effect创建器，比如调用一个异步函数，发起一个action到Store，启动一个后台任务或者等待一个满足某些条件的未来的 action

# redux-saga框架核心API

**一、Saga 辅助函数**

redux-saga提供了一些辅助函数，用来在一些特定的action 被发起到Store时派生任务，下面我先来讲解两个辅助函数：`takeEvery` 和 `takeLatest`

- takeEvery

例如：每次点击 Fetch 按钮时，我们发起一个 FETCH_REQUESTED 的 action。 我们想通过启动一个任务从服务器获取一些数据，来处理这个action

首先我们创建一个将执行异步 action 的任务：

```tsx
import { call, put } from 'redux-saga/effects'

export function* fetchData(action) {
   try {
      const data = yield call(Api.fetchUser, action.payload.url);
      yield put({type: "FETCH_SUCCEEDED", data});
   } catch (error) {
      yield put({type: "FETCH_FAILED", error});
   }
}
```

然后在每次 FETCH_REQUESTED action 被发起时启动上面的任务

```jsx
import { takeEvery } from 'redux-saga'

function* watchFetchData() {

  yield* takeEvery("FETCH_REQUESTED", fetchData)
}
```

**注意**：上面的 takeEvery 函数可以使用下面的写法替换

```jsx
function* watchFetchData() {
  
   while(true){
     yield take('FETCH_REQUESTED');
     yield fork(fetchData);
   }
}
```

- takeLatest

在上面的例子中，takeEvery 允许多个 fetchData 实例同时启动，在某个特定时刻，我们可以启动一个新的 fetchData 任务， 尽管之前还有一个或多个 fetchData 尚未结束

如果我们只想得到最新那个请求的响应（例如，始终显示最新版本的数据），我们可以使用 takeLatest 辅助函数

```jsx
import { takeLatest } from 'redux-saga'

function* watchFetchData() {
  yield* takeLatest('FETCH_REQUESTED', fetchData)
}
```

和takeEvery不同，在任何时刻 takeLatest 只允许执行一个 fetchData 任务，并且这个任务是最后被启动的那个，如果之前已经有一个任务在执行，那之前的这个任务会自动被取消

**二、Effect Creators**

redux-saga框架提供了很多创建effect的函数，下面我们就来简单的介绍下开发中最常用的几种

- take(pattern)
- put(action)
- call(fn, ...args)
- fork(fn, ...args)
- select(selector, ...args)

**take(pattern)**

take函数可以理解为监听未来的action，它创建了一个命令对象，告诉middleware等待一个特定的action， Generator会暂停，直到一个与pattern匹配的action被发起，才会继续执行下面的语句，也就是说，take是一个阻塞的 effect

用法：

```jsx
function* watchFetchData() {
   while(true) {
     // 监听一个type为 'FETCH_REQUESTED' 的action的执行，直到等到这个Action被触发，才会接着执行下面的 yield fork(fetchData)  语句
     yield take('FETCH_REQUESTED');
     yield fork(fetchData);
   }
}
```

**put(action)**

put函数是用来发送action的 effect，你可以简单的把它理解成为redux框架中的dispatch函数，当put一个action后，reducer中就会计算新的state并返回，**注意：** put 也是阻塞 effect

用法：

```jsx
export function* toggleItemFlow() {
    let list = []
    // 发送一个type为 'UPDATE_DATA' 的Action，用来更新数据，参数为 `data：list`
    yield put({
      type: actionTypes.UPDATE_DATA,
      data: list
    })
}
```

**call(fn, ...args)**

call函数你可以把它简单的理解为就是可以调用其他函数的函数，它命令 middleware 来调用fn 函数， args为函数的参数，**注意：** fn 函数可以是一个 Generator 函数，也可以是一个返回 Promise 的普通函数，call 函数也是阻塞 effect

用法：

```jsx
export const delay = ms => new Promise(resolve => setTimeout(resolve, ms))

export function* removeItem() {
  try {
    // 这里call 函数就调用了 delay 函数，delay 函数为一个返回promise 的函数
    return yield call(delay, 500)
  } catch (err) {
    yield put({type: actionTypes.ERROR})
  }
}
```

**fork(fn, ...args)**

fork 函数和 call 函数很像，都是用来调用其他函数的，但是fork函数是非阻塞函数，也就是说，程序执行完 `yield fork(fn， args)` 这一行代码后，会立即接着执行下一行代码语句，而不会等待fn函数返回结果后，在执行下面的语句

用法：

```jsx
import { fork } from 'redux-saga/effects'

export default function* rootSaga() {
  // 下面的四个 Generator 函数会一次执行，不会阻塞执行
  yield fork(addItemFlow)
  yield fork(removeItemFlow)
  yield fork(toggleItemFlow)
  yield fork(modifyItem)
}
```

**select(selector, ...args)**

select 函数是用来指示 middleware调用提供的选择器获取Store上的state数据，你也可以简单的把它理解为redux框架中获取store上的 state数据一样的功能 ：`store.getState()`

用法：

```jsx
export function* toggleItemFlow() {
     // 通过 select effect 来获取 全局 state上的 `getTodoList` 中的 list
     let tempList = yield select(state => state.getTodoList.list)
}
```

**三、createSagaMiddleware()**

createSagaMiddleware 函数是用来创建一个 Redux 中间件，将 Sagas 与 Redux Store 链接起来

sagas 中的每个函数都必须返回一个 Generator 对象，middleware 会迭代这个 Generator 并执行所有 yield 后的 Effect（Effect 可以看作是 redux-saga 的任务单元）

用法：

```jsx
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'
import reducers from './reducers'
import rootSaga from './rootSaga'

// 创建一个saga中间件
const sagaMiddleware = createSagaMiddleware()

// 创建store
const store = createStore(
  reducers,
  将sagaMiddleware 中间件传入到 applyMiddleware 函数中
  applyMiddleware(sagaMiddleware)
)

// 动态执行saga，注意：run函数只能在store创建好之后调用
sagaMiddleware.run(rootSaga)

export default store
```

**四、middleware.run(sagas, ...args)**

动态执行sagas，用于applyMiddleware阶段之后执行sagas

- sagas: Function: 一个 Generator 函数
- args: Array: 提供给 saga 的参数 (除了 Store 的 getState 方法)

注意：动态执行saga语句 `middleware.run(sagas)` 必须要在store创建好之后才能执行，在 store 之前执行，程序会报错

# 以CounterApp Demo来看redux-saga具体使用方式

**index.js **

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'

import rootSaga from './sagas'
import Counter from './Counter'
import rootReducer from './reducers'

const sagaMiddleware = createSagaMiddleware()
let middlewares = []
middlewares.push(sagaMiddleware)

const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore)
const store = createStoreWithMiddleware(rootReducer)

sagaMiddleware.run(rootSaga)

const action = type => store.dispatch({ type })

function render() {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() => action('INCREMENT')}
      onDecrement={() => action('DECREMENT')}
      onIncrementAsync={() => action('INCREMENT_ASYNC')} />,
    document.getElementById('root')
  )
}

render()

store.subscribe(render)
```

**sagas.js**

```jsx
import { put, call, take,fork } from 'redux-saga/effects';
import { takeEvery, takeLatest } from 'redux-saga'

export const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

function* incrementAsync() {
  // 延迟 1s 在执行 + 1操作
  yield call(delay, 1000);
  yield put({ type: 'INCREMENT' });
}

export default function* rootSaga() {
  // while(true){
  //   yield take('INCREMENT_ASYNC');
  //   yield fork(incrementAsync);
  // }

  // 下面的写法与上面的写法上等效
  yield* takeEvery("INCREMENT_ASYNC", incrementAsync)
}
```

**reducer.js**

```jsx
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case 'INCREMENT_ASYNC':
      return state
    default:
      return state
  }
}
```

从上面的代码结构可以看出，redux-saga的使用方式还是比较简单的，相比较之前的redux框架的CounterApp，多了一个sagas的文件，reducers文件还是之前的使用方式

# 总结

本文所讲解的基本上是redux-saga框架在开发中最常使用到的常用API,还有很多不常用API，请参照redux-saga官方文档：http://leonshi.com/redux-saga-in-chinese/docs/api/index.html

如果同学们看到文章这里，还是对redux-saga框架的基本使用不熟悉，概念模糊，建议看看作者提供的Demo示例

**作者建议**：同学们可以将作者之前讲解的[redux框架](https://www.jianshu.com/p/faa98d8bd3fa)和redux-saga框架对比来学习理解，这样更清楚他们之间的区别和联系。

# 更多文章

- 作者React Native开源项目OneM【500+ star】地址(按照企业开发标准搭建框架完成开发的)：**https://github.com/guangqiang-liu/OneM**：欢迎小伙伴们 **star**
- 作者简书主页：包含60多篇RN开发相关的技术文章[http://www.jianshu.com/u/023338566ca5](https://www.jianshu.com/u/023338566ca5)欢迎小伙伴们：**多多关注**，**多多点赞**
- 作者React Native QQ技术交流群：**620792950** 欢迎小伙伴进群交流学习
- 友情提示：**在开发中有遇到RN相关的技术问题，欢迎小伙伴加入交流群（**620792950**），在群里提问、互相交流学习。交流群也定期更新最新的RN学习资料给大家，谢谢大家支持！**