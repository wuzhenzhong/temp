# compose()

- 贡献者1人

  

从右向左编写功能。

这是一个函数式编程实用程序，作为一种便利，它包含在 Redux 中。

您可能想要使用它来连续应用多个存储增强器。

#### 参数

（*参数*）：构成的函数。预计每个功能都可以接受一个参数。它的返回值将作为站立在左边的函数的参数提供，等等。例外是可以接受多个参数的最右边的参数，因为它将提供最终组合函数的签名。

#### 返回

（*function*）：通过从右向左组合给定功能获得的最终function。

#### 示例

这个例子演示了如何在 [redux-devtools ](https://github.com/gaearon/redux-devtools)包内使用`compose`来增强存储`applyMiddleware`和一些开发者工具。

```javascript
import { createStore, combineReducers, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers/index'

const store = createStore(
  reducer,
  compose(
    applyMiddleware(thunk),
    DevTools.instrument()
  )
)
```

#### 提示

- 所有的`compose`功能都可以让你编写深度嵌套的函数转换，而不会让代码向右移动。不要给它太多的信用！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18