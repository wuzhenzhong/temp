# 上手redux中间件，有这篇文章就够了！

0.2312019.07.06 11:59:24字数 3486阅读 240

用过react的同学都知道在redux的存在，redux就是一种前端用来存储数据的仓库，并对改仓库进行增删改查操作的一种框架，它不仅仅适用于react，也使用于其他前端框架。研究过redux源码的人都觉得该源码很精妙，而本博文就针对redux中对中间件的处理进行介绍。

在讲redux中间件之前，先用两张图来大致介绍一下redux的基本原理：





![img](https://upload-images.jianshu.io/upload_images/18616547-35a9f5f3f9956a6b.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

381423828-5ac4fa1bdaad0_articlex.png



图中就是redux的基本流程，这里就不细说。

一般在react中不仅仅利用redux，还利用到react-redux：





![img](https://upload-images.jianshu.io/upload_images/18616547-944d726ad8e39d86.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

1968902651-5ac5014772c5e_articlex.png



由于工作中一直用封装好的如redux-logger、redux-thunk、redux-saga此类的中间件，并没有深入去了解过redux中间件的实现方式。正好最近有个分享，于是就开始着手，于是借此了解了下Redux Middleware的原理。

# 中间件概念

首先简单提下什么是中间件，简单来讲，Redux middleware 提供了一个分类处理 action 的机会。在 middleware 中，我们可以检阅每一个流过的 action,并挑选出特定类型的 action 进行相应操作，以此来改变 action。这样说起来可能会有点抽象，我们直接来看图，这是在没有有中间件和没有中间件情况下的 redux 的数据流：





![img](https://upload-images.jianshu.io/upload_images/18616547-243ae0a22d40d910.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/860/format/webp)

1562386551(1).jpg

不难发现：

1.不使用middleware时，在dispatch(action)时会执行rootReducer，并根据action的type更新返回相应的state。
2.而在使用middleware时，middleware会将我们当前的action做相应的处理。在增加了 middleware 后，我们就可以在这途中对 action 进行截获，并进行改变。随后将新的action再交给其他中间件处理，最后产生新的action给rootReducer执行。且由于业务场景的多样性，单纯的修改 dispatch 和 reduce 人显然不能满足大家的需要，因此对 redux middleware 的设计是可以自由组合，自由插拔的插件机制。也正是由于这个机制，我们在使用 middleware 时，我们可以通过串联不同的 middleware 来满足日常的开发，每一个 middleware 都可以处理一个相对独立的业务需求且相互串联：





![img](https://upload-images.jianshu.io/upload_images/18616547-d0b9006eddc06fa4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

4116027-993b5e2ebc72a5a11.png



如上图所示，派发给 redux Store 的 action 对象，会被 Store 上的多个中间件依次处理，如果把 action 和当前的 state 交给 reducer 处理的过程看做默认存在的中间件，那么其实所有的对 action 的处理都可以有中间件组成的。值得注意的是这些中间件会按照指定的顺序一次处理传入的 action，只有排在前面的中间件完成任务之后，后面的中间件才有机会继续处理 action，同样的，每个中间件都有自己的“熔断”处理,当它认为这个 action 不需要后面的中间件进行处理时，后面的中间件也就不能再对这个 action 进行处理了。
而不同的中间件之所以可以组合使用，是因为 Redux 要求所有的中间件必须提供统一的接口，每个中间件的尉氏县逻辑虽然不一样，但只要遵循统一的接口就能和redux以及其他的中间件对话了。

截止到这我们已经了解了中间件的基本原理了～那么中间件在项目中该如何使用呢？

# 引入ApplyMiddleware高阶函数和需要的中间件实例，并将实例作为ApplyMiddleware的参数注入store。中间件就可以使用了

```jsx
import logger from 'redux-logger';//引入logger中间件
import createSagaMiddleware from 'redux-saga';引入saga中间件构造函数
import mySaga from './saga.js'; 引入要执行的saga文件
import todos from './reducer/reducer';
const sagaMiddleware = createSagaMiddleware();//生成saga实例
const store = createStore(todos, applyMiddleware(logger, sagaMiddleware));//将项目中用到的中间件以参数的形式，通过applyMiddleware函数引入项目。
```

了解了基本原理能有助于我们更快地读懂middleware的源码。业务中，一般我们会这样添加中间件并使用。

```css
createStore(rootReducer, applyMiddleware.apply(null, [...middlewares]))
```

接下来我们可以重点关注这两个函数createStore、applyMiddleware

### CreateStore

```jsx
// 摘至createStore
export function createStore(reducer, rootState, enhance) {
    ...
    if (typeof enhancer !== 'undefined') {
        if (typeof enhancer !== 'function') {
          throw new Error('Expected the enhancer to be a function.')
        }
    /*
        若使用中间件，这里 enhancer 即为 applyMiddleware()
        若有enhance，直接返回一个增强的createStore方法，可以类比成react的高阶函数
    */
    return enhancer(createStore)(reducer, preloadedState)
  }
...
}
```

### ApplyMiddleware

中间件方式中核心部分就是redux提供的applyMiddleWare这个高阶函数，它通过多层调用后悔返回一个全新的store对象，全新的store对象和原来对象中，唯一的不同就是dispatch具备了异步的功能；
源码：

```jsx
export default function applyMiddleware(...middlewares) {
  return createStore => (...args) => {
    // 利用传入的createStore和reducer和创建一个store
    const store = createStore(...args)
    let dispatch = () => {
      throw new Error(
      )
    }
    const middlewareAPI = {
      getState: store.getState,
      dispatch: (...args) => dispatch(...args)
    }
    // 让每个 middleware 带着 middlewareAPI 这个参数分别执行一遍
    const chain = middlewares.map(middleware => middleware(middlewareAPI))
    // 接着 compose 将 chain 中的所有匿名函数，组装成一个新的函数，即新的 dispatch
    dispatch = compose(...chain)(store.dispatch)
    return {
      ...store,
      dispatch
    }
  }
}
```

短短十几行代码，其中却蕴含着不少精妙之处，我选择了其中三处地方进行分析其精妙之处:
1）MiddleWareAPI主要是通过塞进中间件，从而最终塞进action中，让action能具备dispatch的能力，而这里为什么要用匿名函数，主要原因是因为要让MiddleWareAPI.dispatch中的store和applyMiddleWare最终返回的store保持一致，要注意的是MiddleWareAPI.dispatch不是真正让state改变，它可以理解为是action和中间件的一个桥梁。

2）改地方就是将MiddleWareAPI塞进所有的中间件中，然后返回一个函数，而中间件的形式后面会说到。

3）该地方是最为精妙之处，compose会将chain数组从右到左一次地柜注入到前一个中间件，而store.dispatch会注入到最右边的一个的中间件。其实这里可以将compose理解为reduce函数。
eg：

M = [M1,M2,M3] ----> M1(M2(M3(store.dispatch)));
从这里其实就知道中间件大致是什么样子的了：
中间件基本形式：

```jsx
const MiddleWare = (dispatch, getState) => store => next => action => {
    if (typeof action === 'function') {
        ...//action为函数时执行该中间件该有的操作
    } else {
        next(action);
    }
}
```

上面这个函数接受一个对象作为参数，对象的参数上有两个字段 dispatch 和 getState，分别代表着 Redux Store 上的两个同名函数，但需要注意的是并不是所有的中间件都会用到这两个函数。然后 doNothingMidddleware 返回的函数接受一个 next 类型的参数，这个 next 是一个函数，如果调用了它，就代表着这个中间件完成了自己的职能，并将对 action 控制权交予下一个中间件。但需要注意的是，这个函数还不是处理 action 对象的函数，它所返回的那个以 action 为参数的函数才是。最后以 action 为参数的函数对传入的 action 对象进行处理，在这个地方可以进行操作，比如：

调动dispatch派发一个新 action 对象
调用 getState 获得当前 Redux Store 上的状态
调用 next 告诉 Redux 当前中间件工作完毕，让 Redux 调用下一个中间件
访问 action 对象 action 上的所有数据。

在具有上面这些功能后，一个中间件就足够获取 Store 上的所有信息，也具有足够能力可用之数据的流转。看完上面这个最简单的中间件，下面我们来看一下 redux 中间件内，最出名的中间件 redux-thunk 的实现：

```jsx
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
    return next(action);
  };
}
const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;
export default thunk;
```

redux-thunk 中间件的功能也很简单。首先检查参数 action 的类型，如果是函数的话，就执行这个 action 韩湖水，并把 dispatch, getState, extraArgument 作为参数传递进去，否则就调用 next 让下一个中间件继续处理 action 。流程如下：





![img](https://upload-images.jianshu.io/upload_images/18616547-7d4fcbad3b76376c.png?imageMogr2/auto-orient/strip|imageView2/2/w/645/format/webp)

794743064-5ac623cabd6bb_articlex.png

# 异步的中间件

##### 1.redux-thunk

上面说到的redux-thunk就是一个异步的中间件。它通过多参数的 currying 以实现对函数的惰性求值，从而将同步的 action 转为异步的 action。在理解了redux-thunk后，我们在实现数据请求时，action就可以这么写了：

```tsx
function getWeather(url, params) {
    return (dispatch, getState) => {
        fetch(url, params)
            .then(result => {
                dispatch({
                    type: 'GET_WEATHER_SUCCESS', payload: result,
                });
            })
            .catch(err => {
                dispatch({
                    type: 'GET_WEATHER_ERROR', error: err,
                });
            });
        };
}
```

##### redux-thunk的缺点

hunk的缺点也是很明显的，thunk仅仅做了执行这个函数，并不在乎函数主体内是什么，也就是说thunk使
得redux可以接受函数作为action，但是函数的内部可以多种多样。比如下面是一个获取商品列表的异步操作所对应的action：

```jsx
export default ()=>(dispatch)=>{
    fetch('/api/goodList',{ //fecth返回的是一个promise
      method: 'get',
      dataType: 'json',
    }).then(function(json){
      var json=JSON.parse(json);
      if(json.msg==200){
        dispatch({type:'init',data:json.data});
      }
    },function(error){
      console.log(error);
    });
};
```

从这个具有副作用的action中，我们可以看出，函数内部极为复杂。如果需要为每一个异步操作都如此定义一个action，显然action不易维护。

action不易维护的原因：

1.action的形式不统一
2.就是异步操作太为分散，分散在了各个action中
尽管redux-thunk很简单，而且也很实用，但人总是有追求的，都追求着使用更加优雅的方法来实现redux异步流的控制，这就有了redux-saga。

##### 2.redux-saga

redux-saga是一个用于管理redux应用异步操作的中间件，redux-saga通过创建sagas将所有异步操作逻辑收集在一个地方集中处理，可以用来代替redux-thunk中间件。

首先来看redux-thunk的大体过程：

> action1(side function)—>redux-thunk监听—>执行相应的有副作用的方法—>action2(plain object)
> 而redux-saga的大体过程如下：

> action1(plain object)——>redux-saga监听—>执行相应的Effect方法——>返回描述对象—>恢复执行异步和副作用函数—>action2(plain object)

redux-saga中最大的特点就是提供了声明式的Effect，声明式的Effect使得redux-saga监听原始js对象形式的action，并且可以方便单元测试，我们一一来看。

##### Effects

一个effect就是一个纯文本javascript对象，包含一些将被saga middleware执行的指令。
如何创建 effect ?
使用redux-saga提供的 工厂函数 来创建effect
比如：

```csharp
你可以使用  let res = yield call(myfunc,  'arg1', 'arg2')  指示middleware调用  myfunc('arg1', 'arg2')

并将结果返回给 yield 了 effect  的那个 res
```

##### Effect提供的具体方法

首先，在redux-saga中提供了一系列的api，比如take、put、all、select等API ，在redux-saga中将这一系列的api都定义为Effect。这些Effect执行后，当函数resolve时返回一个描述对象，然后redux-saga中间件根据这个描述对象恢复执行generator中的函数。
下面来介绍几个Effect中常用的几个方法，从低阶的API，比如take，call(apply)，fork，put，select等，以及高阶API，比如takeEvery和takeLatest等，从而加深对redux-saga用法的认识
引入：

```csharp
import {take,call,put,select,fork,takeEvery,takeLatest} from 'redux-saga/effects'
```

> take

take这个方法，是用来监听action，返回的是监听到的action对象。比如：

```go
const loginAction = {
   type:'login'
}
```

在UI Component中dispatch一个action:

```undefined
dispatch(loginAction)
```

在saga中使用：

```csharp
const action = yield take('login');
```

可以监听到UI传递到中间件的Action,上述take方法的返回，就是dipath的原始对象。一旦监听到login动作，返回的action为：

```css
{
  type:'login'
}
```

> call(apply)

call和apply方法与js中的call和apply相似，我们以call方法为例：

```css
call(fn, ...args)
```

call方法调用fn，参数为args，返回一个描述对象。不过这里call方法传入的函数fn可以是普通函数，也可以是generator。call方法应用很广泛，在redux-saga中使用异步请求等常用call方法来实现。

```csharp
yield call(fetch,'/userInfo',username)
```

> put

在前面提到，redux-saga做为中间件，工作流是这样的：

> UI——>action1————>redux-saga中间件————>action2————>reducer..

从工作流中，我们发现redux-saga执行完副作用函数后，必须发出action，然后这个action被reducer监听，从而达到更新state的目的。相应的这里的put对应与redux中的dispatch，工作流程图如下：



![img](https://upload-images.jianshu.io/upload_images/18616547-1a1a04ba4da08ed9.png?imageMogr2/auto-orient/strip|imageView2/2/w/718/format/webp)

4116027-0cf60f03d1f7aaaf.png

从图中可以看出redux-saga执行副作用方法转化action时，put这个Effect方法跟redux原始的dispatch相似，都是可以发出action，且发出的action都会被reducer监听到。put的使用方法：

```css
 yield put({type:'login'})
```

> select

put方法与redux中的dispatch相对应，同样的如果我们想在中间件中获取state，那么需要使用select。select方法对应的是redux中的getState，用户获取store中的state，使用方法：

```csharp
const state= yield select()
```

> fork

fork方法相当于web work，fork方法不会阻塞主线程，在非阻塞调用中十分有用。

> takeEvery和takeLatest

takeEvery和takeLatest用于监听相应的action并执行相应的方法，是构建在take和fork上面的高阶api，比如要监听某个或者某几个action，好用takeEvery方法可以：

```bash
takeEvery('acticonType',loginFunc)
```

takeEvery监听到某个actiontype为函数参数中的actionType的action，就会执行loginFunc方法，除此之外，takeEvery可以同时监听到多个相同的action。

takeLatest方法跟takeEvery是相同方式调用：

```bash
takeLatest('login',loginFunc)
```

与takeLatest不同的是，takeLatest是会监听执行最近的那个被触发的action。

> createSagaMiddleware(...sagas)
> createSagaMiddleware的作用是创建一个redux中间件，并将sagas与Redux store建立链接

参数是一个数组，里面是generator函数列表
sagas: Array ---- ( generator函数列表 )

> middleware.run(saga, ...args)

动态执行 saga。用于 applyMiddleware 阶段之后执行 Sagas。这个方法返回一个
Task 描述对象。

saga: Function: 一个 Generator 函数
args: Array: 提供给 saga 的参数 (除了 Store 的 getState 方法)

# redux saga获取异步数据实际操作

##### 1.安装redux saga

```csharp
npm install redux-saga -S
//或者
yarn add redux-saga 
```

##### 2.引入并配置saga

```jsx
import React from 'react';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux'; //引入生成中间件的工厂函数
import createSagaMiddleware from 'redux-saga';//引入saga
import mySaga from './saga.js'; //新建一个文件，在这个文件中编写saga
import todos from './reducer/reducer';
const sagaMiddleware = createSagaMiddleware();//生成saga实例
const store = createStore(todos, applyMiddleware(sagaMiddleware));//将saga注入store
sagaMiddleware.run(mySaga);动态执行 saga。用于 applyMiddleware 阶段之后执行 Sagas。这个方法返回一个Task 描述对象。
```

2.ui组件触发action创建函数

```jsx
class Saga extends Component {
    constructor (props) {
        super(props);
        this.state = {    
        }
        this.handleClickGet = this.handleClickGet.bind(this);
    }
    handleClickGet() {
      this.props.dispatch(getData());
    }
    render () {
        let {num} = this.props.state;
        return (
            <div className="saga"  onClick={this.handleClickGet}>
              异步
            </div>
        )
    }
}
```

3.action创建函数，返回action ----> 传入saga

```tsx
const getData = () => ({
    type: 'GET_DATA',
});
```

4.新建并编写saga文件 ------> 捕获action创建函数返回的action

```jsx
import { call, put, takeEvery } from 'redux-saga/effects'; // 引入相关函数

function* getGitData(action) { // 参数是action创建函数返回的action
    const fn = function() {
        return fetch(`https://api.github.com/users/github`, {
                method: 'GET'
            })
            .then(res => res.json())
            .then(res => {
                return res
            })
    }
    const res = yield call(fn) // 执行p函数，返回值赋值给res

    yield put({ // dispatch一个action到reducer， payload是请求返回的数据
        type: 'GET_DATA_SUCCESS',
        payload: res
    })
}

function* mySaga() { // 在store.js中，执行了 sagaMiddleware.run(rootSaga)
    yield takeEvery('GET_DATA', getGitData) // 如果有对应type的action触发，就执行goAge()函数
}

export default mySaga; // 导出rootSaga，被store.js文件import
```

项目源码：[https://github.com/zhou111222/car](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fzhou111222%2Fcar)

参考文章：
https://www.jianshu.com/p/6f96bdaaea22
[https://www.zcfy.cc/article/async-operations-using-redux-saga-freecodecamp-2377.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.zcfy.cc%2Farticle%2Fasync-operations-using-redux-saga-freecodecamp-2377.html)