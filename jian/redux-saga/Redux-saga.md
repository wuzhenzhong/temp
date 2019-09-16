# Redux-saga

2018.04.18 09:49:39字数 1950阅读 31571

# Redux-saga

# 概述

redux-saga是一个用于管理redux应用异步操作的中间件，redux-saga通过创建sagas将所有异步操作逻辑收集在一个地方集中处理，可以用来代替redux-thunk中间件。

- 这意味着应用的逻辑会存在两个地方
  (1) reducer负责处理action的state更新
  (2) sagas负责协调那些复杂或者异步的操作
- sagas是通过generator函数来创建的
- sagas可以被看作是在后台运行的进程。sagas监听发起的action，然后决定基于这个action来做什么 `(比如：是发起一个异步请求，还是发起其他的action到store，还是调用其他的sagas 等 )`
- 在redux-saga的世界里，所有的任务都通过用 yield Effects 来完成 `( effect可以看作是redux-saga的任务单元 )`
- Effects 都是简单的 javascript对象，包含了要被 saga middleware 执行的信息
- redux-saga 为各项任务提供了各种 （ Effects创建器 )
- 因为使用了generator函数，redux-saga让你可以用 同步的方式来写异步代码
- redux-saga启动的任务可以在任何时候通过手动来取消，也可以把任务和其他的Effects放到 race 方法里以自动取消

------



![img](https://upload-images.jianshu.io/upload_images/6493119-f4c44eb340874014.png?imageMogr2/auto-orient/strip|imageView2/2/w/454/format/webp)

React+Redux Cycle(来源：https://www.youtube.com/watch?v=1QI-UE3-0PU)

- produce: 生产
- flow: 流动，排出
- 整个流程：ui组件触发action创建函数 ---> action创建函数返回一个action ------> action被传入redux中间件(被 saga等中间件处理) ，产生新的action，传入reducer-------> reducer把数据传给ui组件显示 -----> mapStateToProps ------> ui组件显示

------

------

------

------

------

# 安装

```csharp
yarn add redux-saga 
// cnpm install redux-saga -S
```

# 实例

(1) 配置

```jsx
store.js


import {createStore,combineReducers, applyMiddleware} from 'redux';
import userNameReducer from '../username/reducer.js';
import createSagaMiddleware from 'redux-saga';       // 引入redux-saga中的createSagaMiddleware函数
import rootSaga from './saga.js';                    // 引入saga.js

const sagaMiddleware = createSagaMiddleware()        // 执行

const reducerAll = {
    userNameReducer: userNameReducer
}


export const store = createStore(
    combineReducers({...reducerAll}),               // 合并reducer
    window.devToolsExtension ? window.devToolsExtension() : undefined,    // dev-tools
    applyMiddleware(sagaMiddleware)                 // 中间件，加载sagaMiddleware
)

sagaMiddleware.run(rootSaga)                        // 执行rootSaga
```

(2) ui组件触发action创建函数

```jsx
username.js



import React from 'react';

export default class User extends React.Component {
    componentDidMount() {
        console.log(this.props)
    }
    go = () => {
        this.props.getAges(3)           // 发起action，传入参数
    }
    render() {
        return (
            <div>
                这是username组件
                <div>
                    {
                        this.props.username.userNameReducer.username
                    }
                </div>
                <br/>
                <div onClick={this.go}>
                    点击获取提交age
                </div>
            </div>
        )
    }
}
```

(3) action创建函数，返回action ----> 传入saga ( 如果没有saga,就该传入reducer )

```jsx
action.js



import { actionTypes } from './constant.js';

export function getAges(age) {
    console.log(age,'age') // 3
    return {
        type: actionTypes.GET_AGE,
        payload: age
    }
}
```

(4) saga.js ------> 捕获action创建函数返回的action

```jsx
saga.js




import { actionTypes } from '../username/constant.js';
import {call, put, takeEvery} from 'redux-saga/effects';     // 引入相关函数

function* goAge(action){    // 参数是action创建函数返回的action
    console.log(action)
    const p = function() {
        return fetch(`http://image.baidu.com/channel/listjson?rn=${action.payload}...`,{
            method: 'GET'
        })
        .then(res => res.json())
        .then(res => {
            console.log(res)
            return res
        })
    }
    const res = yield call(p)    // 执行p函数，返回值赋值给res

    yield put({      // dispatch一个action到reducer， payload是请求返回的数据
        type: actionTypes.GET_AGE_SUCCESS,
        payload: res   
    })
}

function* rootSaga() {     // 在store.js中，执行了 sagaMiddleware.run(rootSaga)
    yield takeEvery(actionTypes.GET_AGE, goAge)   // 如果有对应type的action触发，就执行goAge()函数
}

export default rootSaga;      // 导出rootSaga，被store.js文件import
```

(5) 然后由ui组件从reducer中获取数据，并显示。。。

------

------

------

------

------

------

# 名词解释

# Effect

一个effect就是一个纯文本javascript对象，包含一些将被saga middleware执行的指令。

- 如何创建 effect ?
  使用redux-saga提供的 工厂函数 来创建effect

```csharp
比如：

你可以使用  call(myfunc,  'arg1', 'arg2')  指示middleware调用  myfunc('arg1', 'arg2')

并将结果返回给 yield 了 effect  的那个  generator
```

# Task

一个 task 就像是一个在后台运行的进程，在基于redux-saga的应用程序中，可以同时运行多个task

- 通过 **fork** 函数来创建 **task**

```php
function* saga() {
  ...
  const task = yield fork(otherSaga, ...args)
  ...
}
```

# 阻塞调用 和 非组塞调用

- 阻塞调用
  阻塞调用的意思是： saga 会在 yield 了 effect 后会等待其执行结果返回，结果返回后才恢复执行 generator 中的下一个指令
- 非阻塞调用
  非阻塞调用的意思是： saga 会在 yield effect 之后立即恢复执行

# watcher 和 worker

指的是一种使用两个单独的saga来组织控制流的方式

- watcher：监听发起的action 并在每次接收到action时 **fork** 一个 **work**
- worker： 处理action，并结束它

------

------

------

------

------

# api

# createSagaMiddleware(...sagas)

createSagaMiddleware的作用是创建一个redux中间件，并将sagas与Redux store建立链接

- 参数是一个数组，里面是generator函数列表
- sagas: Array ---- ( `generator函数列表` )

# middleware.run(saga, ...args)

动态执行 saga。用于 applyMiddleware 阶段之后执行 Sagas。这个方法返回一个
Task 描述对象。

- saga: Function: 一个 Generator 函数
- args: Array: 提供给 saga 的参数 (除了 Store 的 getState 方法)

------

# take(pattern)

# ----- 暂停Generator，匹配的action被发起时，恢复执行

创建一条 Effect 描述信息，指示 middleware 等待 Store 上指定的 action。 Generator 会暂停，直到一个与 pattern 匹配的 action 被发起。
pattern的规则
(1) pattern为空 或者 * ，将会匹配所有发起的action

(2) pattern是一个函数，action 会在 pattern(action) 返回为 true 时被匹配
`（例如，take(action => action.entities) 会匹配那些 entities 字段为真的 action）。`

**(3) pattern是一个字符串，action 会在 action.type === pattern 时被匹配**

(4) pattern是一个数组，会针对数组所有项，匹配与 action.type 相等的 action
`（例如，take([INCREMENT, DECREMENT]) 会匹配 INCREMENT 或 DECREMENT 类型的 action）`

take实例：

```jsx
username.js



import React from 'react';


export default class User extends React.Component {
    go = () => {
        new Promise((resolve,reject) => {
            resolve(3)
        }).then(res => this.props.getAges(res))    // 执行action.js中的getAges函数
            .then(res => {
                setTimeout(()=> {
                    console.log('5s钟后才会执行settimeout')
                    this.props.settimeout()
                },5000)           // 在getAges函数执行完后，再过5s执行，settimeout()函数
            }) 
        
        
    }
    render() {
        console.log(this.props, 'this.props')
        return (
            <div>
                这是username组件
                <br/>
                <div onClick={this.go}>
                    点击获取提交age
                </div>
                <div>
                    {
                        this.props.username && 
                        this.props.username.userNameReducer.image.data && 
                        this.props.username.userNameReducer.image.data.map(
                            (item,key) => <div key={key}>{item.abs }</div>
                        )
                    }
                </div>
            </div>
        )
    }
}
action.js



import { actionTypes } from './constant.js';


export function getAges(age) {
    console.log(age,'age')
    return {
        type: actionTypes.GET_AGE,   // 在saga中有对应的actionTypes.GET_AGE
        payload: age
    }
}

export function settimeout() {
    return {
        type: actionTypes.MATCH_TAKE,  // 在saga中有对应的actionTypes.MATCH_TAKE,
    }
}
saga.js



import { actionTypes } from '../username/constant.js';
import {call, put, takeEvery, take} from 'redux-saga/effects';

function* goAge(action){
    console.log(action)
    const p = function() {
        return fetch(
            `http://image.baidu.com/channel/listjson?pn=0&rn=${action.payload}`,{
            method: 'GET'
        })
        .then(res => res.json())
        .then(res => {
            console.log(res)
            return res
        })
    }
    const res = yield call(p)
    yield take(actionTypes.MATCH_TAKE)   

    // generator执行到take时，会暂停执行，直到有type为MATCH_TAKE的action发起时，才恢复执行

    // 这里的效果就是点击按钮 5s钟后 才显示请求到的内容，( 5s钟后才执行下面的put语句 )
    yield put({
        type: actionTypes.GET_AGE_SUCCESS,
        payload: res
    })
}

function* rootSaga() {
    yield takeEvery(actionTypes.GET_AGE, goAge)  
          
    // 有对应的type是GET_AGE的action发起时，执行goAge() Generator函数
}

export default rootSaga;
```

------

# fork(fn, ...args)

# ----- 无阻塞的执行fn，执行fn时，不会暂停Generator

# ----- yield fork(fn ...args)的结果是一个 [Task](https://redux-saga-in-chinese.js.org/docs/api/index.html#task) 对象

task对象 ---------- 一个具备某些有用的方法和属性的对象

创建一条 Effect 描述信息，指示 middleware 以 无阻塞调用 方式执行 fn。

- fn: Function - 一个 Generator 函数, 或者返回 Promise 的普通函数
- args: Array - 一个数组，作为 fn 的参数
- `fork` 类似于 `call`，可以用来调用普通函数和 Generator 函数。但 fork 的调用是无阻塞的，在等待 fn 返回结果时，middleware 不会暂停 Generator。 相反，一旦 fn 被调用，Generator 立即恢复执行。
- `fork` 与 `race` 类似，是一个中心化的 Effect，管理 Sagas 间的并发。
  `yield fork(fn ...args)` 的结果是一个 [Task](https://redux-saga-in-chinese.js.org/docs/api/index.html#task) 对象 —— 一个具备某些有用的方法和属性的对象。
- fork: 是分叉，岔路的意思 ( 并发 )

实例：

```jsx
import { actionTypes } from '../username/constant.js';
import {call, put, takeEvery, take, fork} from 'redux-saga/effects';

function* goAge(action){

    function* x() {
        yield setTimeout(() => {
           console.log('该显示会在获得图片后，2s中后显示') 
        }, 2000);
    }

    const p = function() {
        return fetch(`http://image.baidu.com/channel/listjson?pn=0&rn=${action.payload}`,{
            method: 'GET'
        })
        .then(res => res.json())
        .then(res => {
            console.log(res)
            return res
        })
    }
    const res = yield call(p)

    yield take(actionTypes.MATCH_TAKE)   // 阻塞，直到匹配的action触发，才会恢复执行

    yield fork(x)  // 无阻塞执行，即x()generator触发后，就会执行下面的put语句

    yield put({
        type: actionTypes.GET_AGE_SUCCESS,
        payload: res
    })

}

function* rootSaga() {
    yield takeEvery(actionTypes.GET_AGE, goAge)
}

export default rootSaga;
```

------

# join(task)

# ----- 等待fork任务返回结果(task对象)

创建一条 Effect 描述信息，指示 middleware 等待之前的 fork 任务返回结果。

- `task: Task` - 之前的 `fork` 指令返回的 [Task](https://redux-saga-in-chinese.js.org/docs/api/index.html#task) 对象

- `yield fork(fn, ...args)` 返回的是一个 task 对象

------

# cancel(task)

创建一条 Effect 描述信息，指示 middleware 取消之前的 fork 任务。

- `task: Task` - 之前的 `fork` 指令返回的 [Task](https://redux-saga-in-chinese.js.org/docs/api/index.html#task) 对象

- cancel 是一个无阻塞 Effect。也就是说，Generator 将在取消异常被抛出后立即恢复。

------

# select(selector, ...args)

# ----- 得到 Store 中的 state 中的数据

创建一条 Effect 描述信息，指示 middleware 调用提供的选择器获取 Store state 上的数据（例如，返回 selector(getState(), ...args) 的结果）。

- `selector: Function` - 一个 `(state, ...args) => args` 函数. 通过当前 state 和一些可选参数，返回当前 Store state 上的部分数据。
- `args: Array` - 可选参数，传递给选择器（附加在 getState 后）
- **如果 select 调用时参数为空( --- 即 yield select() --- )，那 effect 会取得整个的 state**
  `（和调用 getState() 的结果一样）`

> 重要提醒：在发起 action 到 store 时，middleware 首先会转发 action 到 reducers 然后通知 Sagas。这意味着，当你查询 Store 的 state， 你获取的是 action 被处理之后的 state。

------

# put(action)

# ----- 发起一个 action 到 store

创建一条 Effect 描述信息，指示 middleware 发起一个 action 到 Store。

- `action: Object` - [完整信息可查看 Redux 的 `dispatch` 文档](http://redux.js.org/docs/api/Store.html#dispatch)

- put 是异步的，不会立即发生

------

# call(fn, ...args) 阻塞执行，call()执行完，才会往下执行

# ----- 执行 fn(...args)

# ----- 对比 fork(fn, ...args) 无阻塞执行

创建一条 Effect 描述信息，指示 middleware 调用 fn 函数并以 args 为参数。

fn: Function - 一个 Generator 函数, 或者返回 Promise 的普通函数

args: Array - 一个数组，作为 fn 的参数

- fn 既可以是一个普通函数，也可以是一个 Generator 函数

------

# race(effects)

- effects: Object : 一个`{label: effect, ...}`形式的字典对象

# 同时执行多个任务

当我们需要 yield 一个包含 effects 的数组， generator 会被阻塞直到所有的 effects 都执行完毕，或者当一个 effect 被拒绝 （就像 Promise.all 的行为）时，才会恢复执行Generator函数 ( yield后面的语句 )。

```csharp
import { call } from 'redux-saga/effects'


// 正确写法, effects 将会同步执行
const [users, repos] = yield [
  call(fetch, '/users'),
  call(fetch, '/repos')
]


---------------------------------------------------------------------

// 错误写法
const users = yield call(fetch, '/users'),
const repos = yield call(fetch, '/repos')

// call会阻塞执行，第二个 effect 将会在第一个 call 执行完毕才开始。
```

------

------

2018-6-25

# throttle ------ 节流阀的意思

throttle(ms, pattern, saga, ...args)

- 用途，是在处理任务时，无视给定的时长内新传入的 action。即在指定的时间内，只执行一次 saga函数
  实例：

```jsx
import {put, call, takeEvery, select, fork, all, throttle } from 'redux-saga/effects';
import axios from 'axios';


const request = function(data) {
    return axios('/channel/listjson?pn=0&rn=30&tag1=明星&tag2=全部&ie=utf8', {
        params: data
    });
}


const getSagaImage = function* (data) {
    try {
        const res = yield call(request, data);
        const datas = res.data.data ?  res.data.data : null
        yield put({
            type: 'GET_IMAGE_SUCCESS',
            payload: datas
        });
        const uuid = yield select(state => state.user.username);
        console.log(uuid, 'uuid')
    } catch (err) {
        alert(err.message)
    }
}



const watchThrottle = function* () {
    yield throttle(3000, 'GET_IMAGES', getSagaImage);   // 3s内新的操作将被忽略
}

// const rootSaga =  function* () {
//     yield takeEvery('GET_IMAGES', getSagaImage)
// };

const rootSaga = function* () {
    yield all([
        fork(watchThrottle),
    ])
}

export default rootSaga;
```