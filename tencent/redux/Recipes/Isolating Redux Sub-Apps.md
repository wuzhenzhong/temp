# 隔离Redux子应用程序( Isolating Redux Sub-Apps )

- 贡献者1人

  

考虑`<BigApp>`嵌入较小的“子应用程序”（包含在`<SubApp>`组件中）的“大”应用程序（包含在组件中）的情况：

```javascript
import React, { Component } from 'react'
import SubApp from './subapp'

class BigApp extends Component {
  render() {
    return (
      <div>
        <SubApp />
        <SubApp />
        <SubApp />
      </div>
    )
  }
}
```

这些`<SubApp>`将完全独立。他们不会共享数据或行为，也不会看到或彼此沟通。

最好不要将此方法与标准 Redux 还原剂组合物混合。对于典型的 Web 应用程序，坚持减速器组成。对于将不同工具分组为统一软件包的“产品中心”，“仪表板”或企业软件，请尝试使用子应用程序方法。

子应用程序方法对于按产品或功能垂直划分的大型团队也很有用。这些团队可以独立运送子应用程序，或与封闭的“应用程序外壳”结合使用。

下面是一个子应用程序的根连接组件。像往常一样，它可以呈现更多的组件，连接与否，作为孩子。通常我们会在`<Provider>`渲染它并完成它。

```javascript
class App extends Component { ... }
export default connect(mapStateToProps)(App)
```

但是，如果我们有兴趣隐藏子应用程序组件是Redux应用程序的事实，则不必调用`ReactDOM.render(<Provider><App/>` `</Provider>`)。

也许我们希望能够在同一个“更大”的应用程序中运行它的多个实例，并将它保存为一个完整的黑盒子，其中 Redux 是一个实现细节。

要将 Reux 隐藏在 React API 的后面，我们可以将它包装在一个特殊的组件中，该组件可以在构造函数中初始化商店：

```javascript
import React, { Component } from 'react'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import reducer from './reducers'
import App from './App'

class SubApp extends Component {
  constructor(props) {
    super(props)
    this.store = createStore(reducer)
  }

  render() {
    return (
      <Provider store={this.store}>
        <App />
      </Provider>
    )
  }
}
```

这样每个实例都将是独立的。

这种模式*不* 推荐用于共享数据的同一应用程序的某些部分。然而，当更大的应用程序对内部小应用程序的访问权限为零时，它可能很有用，并且我们希望将它们作为实现细节与 Redux 一起实现。每个组件实例都有自己的商店，所以他们不会“彼此了解”。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18