# API参考（API Reference）

- 贡献者1人

  

Redux API 表面很小。Redux 定义了一系列供您实施的合同（例如 reducer ），并提供了一些帮助功能来将这些合同捆绑在一起。

本节介绍完整的Redux API。请记住，Redux只关心管理状态。在一个真正的应用程序中，您还需要使用像[react-redux](https://github.com/gaearon/react-redux)这样的UI绑定。

### 顶级出口

- createStore(reducer, [preloadedState], [enhancer])
- combineReducers(reducers)
- applyMiddleware(...middlewares)
- bindActionCreators(actionCreators, dispatch)
- compose(...functions)

### Store API

- Store
  - getState()
  - dispatch(action)
  - subscribe(listener)
  - replaceReducer(nextReducer)

### 输入

上述每个功能都是顶级导出。你可以像这样导入它们中的任何一个：

#### ES6

```javascript
import { createStore } from 'redux'
```

#### ES5 (CommonJS)

```javascript
var createStore = require('redux').createStore
```

#### ES5 (UMD build)

```javascript
var createStore = Redux.createStore
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18