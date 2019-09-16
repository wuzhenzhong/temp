# 2、Redux与它的中间件：redux-thunk，redux-actions，redux-promise，redux-saga

0.7972019.07.09 16:01:27字数 3637阅读 162

## 序言

这里要讲的就是一个Redux在React中的应用问题，讲一讲Redux，react-redux，redux-thunk，redux-actions，redux-promise，redux-saga这些包的作用和他们解决的问题。
因为不想把篇幅拉得太长，所以没有太多源码分析和语法讲解，能怎么简单就怎么简单。

## Redux

先看看百度百科上面Redux的一张图：



![img](https://upload-images.jianshu.io/upload_images/12693563-a0fe409f3a56d12e.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

这是Redux在Github上的介绍：Redux用于js程序，是一个可预测的状态容器。

在这里我们首先要明白的是什么叫可预测？什么叫状态容器？

什么叫状态？实际上就是变量，对话框显示或隐藏的变量，一杯奶茶多少钱的变量。

那么这个状态容器，实际上就是一个存放这些变量的变量。

你创建了一个全局变量叫Store，然后将代码中控制各个状态的变量存放在里面，那么现在Store就叫做状态容器。

什么叫可预测？

你在操作这个Store的时候，总是用Store.price的方式来设置值，这种操作数据的方式很原始，对于复杂的系统而言永远都不知道程序在运行的过程中发生了什么。

那么现在我们都通过发送一个Action去做修改，而Store在接收到Action后会使用Reducer对Action传递的数据做处理，最后应用到Store中。

相对于Store.price的方式来修改者，这种方式无疑更麻烦，但是这种方式的好处就是，每一个Action里面都可以写日志，可以记录各种状态的变动，这就是可预测。

所以如果你的程序很简单，你完全没有必要去用Redux。

看看Redux的示例代码：

actionTypes.js：

```jsx
export const CHANGE_BTN_TEXT = 'CHANGE_BTN_TEXT';
```

actions.js：

```tsx
import * as T from './actionTypes';

export const changeBtnText = (text) => {
  return {
    type: T.CHANGE_BTN_TEXT,
    payload: text
  };
};
```

reducers.js：

```jsx
import * as T from './actionTypes';

const initialState = {
  btnText: '我是按钮',
};

const pageMainReducer = (state = initialState, action) => {
  switch (action.type) {
    case T.CHANGE_BTN_TEXT:
      return {
        ...state,
        btnText: action.payload
      };
    default:
      return state;
  }
};

export default pageMainReducer;
```

index.js

```jsx
import { createStore } from 'redux';
import reducer from './reducers';
import { changeBtnText } from './actions';

const store = createStore(reducer);
// 开始监听，每次state更新，那么就会打印出当前状态
const unsubscribe = store.subscribe(() => {
  console.info(store.getState());
});
// 发送消息
store.dispatch(changeBtnText('点击了按钮'));
// 停止监听state的更新
unsubscribe();
```

这里就不解释什么语法作用了，网上这样的资料太多了。

## Redux与React的结合：react-redux

Redux是一个可预测的状态容器，跟React这种构建UI的库是两个相互独立的东西。

Redux要应用到React中，很明显action，reducer，dispatch这几个阶段并不需要改变，唯一需要考虑的是redux中的状态需要如何传递给react组件。

很简单，只需要每次要更新数据时运用store.getState获取到当前状态，并将这些数据传递给组件即可。

那么问题来了，如何让每个组件都获取到store呢？

当然是将store作为一个值传递给根组件，然后store就会一级一级往下传，使得每个组件都能获取到store的值。

但是这样太繁琐了，难道每个组件需要写一个传递store的逻辑？为了解决这个问题，那么得用到React的context玩法，通过在根组件上将store放在根组件的context中，然后在子组件中通过context获取到store。

react-redux的主要思路也是如此，通过嵌套组件Provider将store放到context中，通过connect这个高阶组件，来隐藏取store的操作，这样我们就不需要每次去操作context写一大堆代码那么麻烦了。

然后我们再来基于之前的Redux示例代码给出react-redux的使用演示代码，其中action和reduce部分不变，先增加一个组件PageMain：

```jsx
const PageMain = (props) => {
  return (
    <div>
      <button onClick={() => {
        props.changeText('按钮被点击了');
      }}
      >
        {props.btnText}
      </button>
    </div>
  );
};
// 映射store.getState()的数据到PageMain
const mapStateToProps = (state) => {
  return {
    btnText: state.pageMain.btnText,
  };
};
// 映射使用了store.dispatch的函数到PageMain
const mapDispatchToProps = (dispatch) => {
  return {
    changeText: (text) => {
      dispatch(changeBtnText(text));
    }
  };
};

// 这个地方也可以简写，react-redux会自动做处理
const mapDispatchToProps = {
  changeText: changeBtnText
};

export default connect(mapStateToProps, mapDispatchToProps)(PageMain);
```

注意上面的state.pageMain.btnText，这个pageMain是我用redux的combineReducers将多个reducer合并后给的原先的reducer一个命名。

它的代码如下：

```jsx
import { combineReducers } from 'redux';
import pageMain from './components/pageMain/reducers';

const reducer = combineReducers({
  pageMain
});

export default reducer;
```

然后修改index.js:

```jsx
import React from 'react';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import ReactDOM from 'react-dom';
import reducer from './reducers';
import PageMain from './components/pageMain';

const store = createStore(reducer);

const App = () => (
  <Provider store={store}>
    <PageMain />
  </Provider>
);

ReactDOM.render(<App />, document.getElementById('app'));
```

## Redux的中间件

之前我们讲到Redux是个可预测的状态容器，这个可预测在于对数据的每一次修改都可以进行相应的处理和记录。

假如现在我们需要在每次修改数据时，记录修改的内容，我们可以在每一个dispatch前面加上一个console.info记录修改的内容。

但是这样太繁琐了，所以我们可以直接修改store.dispatch：

```jsx
let next = store.dispatch
store.dispatch = (action)=> {
  console.info('修改内容为：', action)
  next(action)
}
```

Redux中也有同样的功能，那就是applyMiddleware。直译过来就是“应用中间件”，它的作用就是改造dispatch函数，跟上面的玩法基本雷同。

来一段演示代码：

```jsx
import { createStore, applyMiddleware } from 'redux';
import reducer from './reducers';

const store = createStore(reducer, applyMiddleware(curStore => next => action => {
  console.info(curStore.getState(), action);
  return next(action);
}));
```

看起来挺奇怪的玩法，但是理解起来并不难。通过这种返回函数的方法，使得applyMiddleware内部以及我们使用时可以处理store和action，并且这里next的应用就是为了使用多个中间件而存在的。

而通常我们没有必要自己写中间件，比如日志的记录就已经有了成熟的中间件：redux-logger，这里给一个简单的例子：

```jsx
import { applyMiddleware, createStore } from 'redux';
import createLogger from 'redux-logger';
import reducer from './reducers';

const logger = createLogger();

const store = createStore(
  reducer,
  applyMiddleware(logger)
);
```

这样就可以记录所有action及其发送前后的state的日志，我们可以了解到代码实际运行时到底发生了什么。

## redux-thunk：处理异步action

在上面的代码中，我们点击按钮后，直接修改了按钮的文本，这个文本是个固定的值。

actions.js：

```tsx
import * as T from './actionTypes';

export const changeBtnText = (text) => {
  return {
    type: T.CHANGE_BTN_TEXT,
    payload: text
  };
};
```

但是在我们实际生产的过程中，很多情况都是需要去请求服务端拿到数据再修改的，这个过程是一个异步的过程。又或者需要setTimeout去做一些事情。

我们可以去修改这一部分如下：

```jsx
const mapDispatchToProps = (dispatch) => {
  return {
    changeText: (text) => {
      dispatch(changeBtnText('正在加载中'));
      axios.get('http://test.com').then(() => {
        dispatch(changeBtnText('加载完毕'));
      }).catch(() => {
        dispatch(changeBtnText('加载有误'));
      });
    }
  };
};
```

实际上，我们每天不知道要处理多少这样的代码。

但是问题来了，异步操作相比同步操作多了一个很多确定因素，比如我们展示正在加载中时，可能要先要做异步操作A，而请求后台的过程却非常快，导致加载完毕先出现，而这时候操作A才做完，然后再展示加载中。

所以上面的这个玩法并不能满足这种情况。

这个时候我们需要去通过store.getState获取当前状态，从而判断到底是展示正在加载中还是展示加载完毕。

这个过程就不能放在mapDispatchToProps中了，而需要放在中间件中，因为中间件中可以拿到store。

首先创造store的时候需要应用react-thunk，也就是

```jsx
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import reducer from './reducers';

const store = createStore(
  reducer,
  applyMiddleware(thunk)
);
```

它的源码超级简单：

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

从这个里面可以看出，它就是加强了dispatch的功能，在dispatch一个action之前，去判断action是否是一个函数，如果是函数，那么就执行这个函数。

那么我们使用起来就很简单了，此时我们修改actions.js

```tsx
import axios from 'axios';
import * as T from './actionTypes';

export const changeBtnText = (text) => {
  return {
    type: T.CHANGE_BTN_TEXT,
    payload: text
  };
};

export const changeBtnTextAsync = (text) => {
  return (dispatch, getState) => {
    if (!getState().isLoading) {
      dispatch(changeBtnText('正在加载中'));
    }
    axios.get(`http://test.com/${text}`).then(() => {
      if (getState().isLoading) {
        dispatch(changeBtnText('加载完毕'));
      }
    }).catch(() => {
      dispatch(changeBtnText('加载有误'));
    });
  };
};
```

而原来mapDispatchToProps中的玩法和同步action的玩法是一样的：

```jsx
const mapDispatchToProps = (dispatch) => {
  return {
    changeText: (text) => {
      dispatch(changeBtnTextAsync(text));
    }
  };
};
```

通过redux-thunk我们可以简单地进行异步操作，并且可以获取到各个异步操作时期状态的值。

## redux-actions：简化redux的使用

Redux虽然好用，但是里面还是有些重复代码，所以有了redux-actions来简化那些重复代码。

这部分简化工作主要集中在构造action和处理reducers方面。

先来看看原先的actions

```tsx
import axios from 'axios';
import * as T from './actionTypes';

export const changeBtnText = (text) => {
  return {
    type: T.CHANGE_BTN_TEXT,
    payload: text
  };
};

export const changeBtnTextAsync = () => {
  return (dispatch, getState) => {
    if (!getState().isLoading) {
      dispatch(changeBtnText('正在加载中'));
    }
    axios.get('http://test.com').then(() => {
      if (getState().isLoading) {
        dispatch(changeBtnText('加载完毕'));
      }
    }).catch(() => {
      dispatch(changeBtnText('加载有误'));
    });
  };
};
```

然后再来看看修改后的：

```tsx
import axios from 'axios';
import * as T from './actionTypes';
import { createAction } from 'redux-actions';

export const changeBtnText = createAction(T.CHANGE_BTN_TEXT, text => text);

export const changeBtnTextAsync = () => {
  return (dispatch, getState) => {
    if (!getState().isLoading) {
      dispatch(changeBtnText('正在加载中'));
    }
    axios.get('http://test.com').then(() => {
      if (getState().isLoading) {
        dispatch(changeBtnText('加载完毕'));
      }
    }).catch(() => {
      dispatch(changeBtnText('加载有误'));
    });
  };
};
```

这一块代码替换上面的部分代码后，程序运行结果依然保持不变，也就是说createAction只是对上面的代码进行了简单的封装而已。

这里注意到，异步的action就不要用createAction，因为这个createAction返回的是一个对象，而不是一个函数，就会导致redux-thunk的代码没有起到作用。

这里也可以使用createActions这个函数同时创建多个action，但是讲道理，这个语法很奇怪，用createAction就好。

同样redux-actions对reducer的部分也进行了处理，比如handleAction以及handelActions。

先来看看原先的reducers

```jsx
import * as T from './actionTypes';

const initialState = {
  btnText: '我是按钮',
};

const pageMainReducer = (state = initialState, action) => {
  switch (action.type) {
    case T.CHANGE_BTN_TEXT:
      return {
        ...state,
        btnText: action.payload
      };
    default:
      return state;
  }
};

export default pageMainReducer;
```

然后使用handleActions来处理

```jsx
import { handleActions } from 'redux-actions';
import * as T from './actionTypes';

const initialState = {
  btnText: '我是按钮',
};

const pageMainReducer = handleActions({
  [T.CHANGE_BTN_TEXT]: {
    next(state, action) {
      return {
        ...state,
        btnText: action.payload,
      };
    },
    throw(state) {
      return state;
    },
  },
}, initialState);

export default pageMainReducer;
```

这里handleActions可以加入异常处理，并且帮助处理了初始值。

注意，无论是createAction还是handleAction都只是对代码做了一点简单的封装，两者可以单独使用，并不是说使用了createAction就必须要用handleAction。

## redux-promise：redux-actions的好基友，轻松创建和处理异步action

还记得上面在使用redux-actions的createAction时，我们对异步的action无法处理。

因为我们使用createAction后返回的是一个对象，而不是一个函数，就会导致redux-thunk的代码没有起到作用。

而现在我们将使用redux-promise来处理这类情况。

可以看看之前我们使用 createAction的例子：

```tsx
export const changeBtnText = createAction(T.CHANGE_BTN_TEXT, text => text);
```

现在我们先加入redux-promise中间件：

```jsx
import thunk from 'redux-thunk';
import createLogger from 'redux-logger';
import promiseMiddleware from 'redux-promise';
import reducer from './reducers';

const store = createStore(reducer, applyMiddleware(thunk, createLogger, promiseMiddleware));
```

然后再处理异步action:

```tsx
export const changeBtnTextAsync = createAction(T.CHANGE_BTN_TEXT_ASYNC, (text) => {
  return axios.get(`http://test.com/${text}`);
});
```

可以看到我们这里返回的是一个Promise对象.(axios的get方法结果就是Promise对象)

我们还记得redux-thunk中间件，它会去判断action是否是一个函数，如果是就执行。

而我们这里的redux-promise中间件，他会在dispatch时，判断如果action不是类似

```bash
{
  type：'',
  payload： ''
}
```

这样的结构，也就是 [FSA](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fredux-utilities%2Fflux-standard-action),那么就去判断是否为promise对象，如果是就执行action.then的玩法。

很明显，我们createAction后的结果是FSA，所以会走下面这个分支，它会去判断action.payload是否为promise对象，是的话那就

```jsx
action.payload
  .then(result => dispatch({ ...action, payload: result }))
  .catch(error => {
    dispatch({ ...action, payload: error, error: true });
    return Promise.reject(error);
  })
```

也就是说我们的代码最后会转变为：

```jsx
axios.get(`http://test.com/${text}`)
  .then(result => dispatch({ ...action, payload: result }))
  .catch(error => {
    dispatch({ ...action, payload: error, error: true });
    return Promise.reject(error);
  })
```

这个中间件的代码也很简单，总共19行，大家可以在github上直接看看。

## redux-saga：控制器与更优雅的异步处理

我们的异步处理用的是redux-thunk + redux-actions + redux-promise，其实用起来还是蛮好用的。

但是随着ES6中Generator的出现，人们发现用Generator处理异步可以更简单。

而redux-saga就是用Generator来处理异步。

以下讲的知识是基于Generator的，如果您对这个不甚了解，可以简单了解一下相关知识，大概需要2分钟时间，并不难。

redux-saga文档并没有说自己是处理异步的工具，而是说用来处理边际效应（side effects），这里的边际效应你可以理解为程序对外部的操作，比如请求后端，比如操作文件。

redux-saga同样是一个redux中间件，它的定位就是通过集中控制action，起到一个类似于MVC中控制器的效果。

同时它的语法使得复杂异步操作不会像promise那样出现很多then的情况，更容易进行各类测试。

这个东西有它的好处，同样也有它不好的地方，那就是比较复杂，有一定的学习成本。

并且我个人而言很不习惯Generator的用法，觉得Promise或者await更好用。

这里还是记录一下用法，毕竟有很多框架都用到了这个。

应用这个中间件和我们的其他中间件没有区别：

```jsx
import React from 'react';
import { createStore, applyMiddleware } from 'redux';
import promiseMiddleware from 'redux-promise';
import createSagaMiddleware from 'redux-saga';
import {watchDelayChangeBtnText} from './sagas';
import reducer from './reducers';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(reducer, applyMiddleware(promiseMiddleware, sagaMiddleware));

sagaMiddleware.run(watchDelayChangeBtnText);
```

创建sage中间件后，然后再将其中间件接入到store中,最后需要用中间件运行sages.js返回的Generator,监控各个action。

现在我们给出sages.js的代码：

```jsx
import { delay } from 'redux-saga';
import { put, call, takeEvery } from 'redux-saga/effects';
import * as T from './components/pageMain/actionTypes';
import { changeBtnText } from './components/pageMain/actions';

const consoleMsg = (msg) => {
  console.info(msg);
};
/**
 * 处理编辑效应的函数
 */
export function* delayChangeBtnText() {
  yield delay(1000);
  yield put(changeBtnText('123'));
  yield call(consoleMsg, '完成改变');
}
/**
 * 监控Action的函数
 */
export function* watchDelayChangeBtnText() {
  yield takeEvery(T.WATCH_CHANGE_BTN_TEXT, delayChangeBtnText);
}
```

在redux-saga中有一类用来处理边际效应的函数比如put、call，它们的作用是为了简化操作。

比如put相当于redux的dispatch的作用，而call相当于调用函数。（可以参考上面代码中的例子）

还有另一类函数就是类似于takeEvery，它的作用就是和普通redux中间件一样拦截到action后作出相应处理。

比如上面的代码就是拦截到T.WATCH_CHANGE_BTN_TEXT这个类型的action，然后调用delayChangeBtnText。

然后可以回看我们之前的代码，有这么一行代码：

sagaMiddleware.run(watchDelayChangeBtnText);

这里实际就是引入监控的这个生成器后，再运行监控生成器。

这样我们在代码里面dispatch类型为T.WATCH_CHANGE_BTN_TEXT的action时就会被拦截然后做出相应处理。

当然这里有人可能会提出疑问，难道每一个异步都要这么写吗，那岂不是要run很多次？

当然不是这个样子，我们可以在sage中这么写：

```jsx
export default function* rootSaga() {
  yield [
    watchDelayChangeBtnText()，
    watchOtherAction()
  ]
}
```

我们只需要按照这个格式去写，将watchDelayChangeBtnText这样用于监控action的生成器放在上面那个代码的数组中，然后作为一个生成器返回。

现在只需要引用这个rootSaga即可，然后run这个rootSaga。

以后如果要监控更多的action，只需要在sages.js中加上新的监控的生成器即可。

通过这样的处理，我们就将sages.js做成了一个像MVC中的控制器的东西，可以用来处理各种各样的action，处理复杂的异步操作和边际效应。

但是这里要注意,一定要加以区分sages.js中使用监控的action和真正功能用的action，比如加个watch关键字，以免业务复杂后代码混乱。

## 总结

总的来说：

- redux是一个可预测的状态容器，
- react-redux是将store和react结合起来，使得数据展示和修改对于react项目而言更简单
- redux中间件就是在dispatch action前对action做一些处理
- redux-thunk用于对异步做操作
- redux-actions用于简化redux操作
- redux-promise可以配合redux-actions用来处理Promise对象，使得异步操作更简单
- redux-saga可以起到一个控制器的作用，集中处理边际效用，并使得异步操作的写法更优雅。