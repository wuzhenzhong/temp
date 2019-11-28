# 使用React路由器（Usage with React Router）

- 贡献者1人

  

所以你想用你的 Redux 应用程序进行路由。你可以用 [React Router ](https://github.com/reactjs/react-router)使用它。Redux 将成为您的数据的真实来源，而 React Router 将成为您的 URL 的真实来源。在大多数情况下，除非您需要时间旅行和倒带触发更改 URL 的操作，否则将它们分开**是很好的做法**。

## 安装React路由器

`react-router`在 npm 上可用。本指南假定您正在使用`react-router@^2.7.0`。

```
npm install --save react-router
```

## 配置Fallback URL

在集成 React Router 之前，我们需要配置我们的开发服务器。事实上，我们的开发服务器可能不知道 React Router 配置中声明的路由。例如，如果您访问`/todos`并刷新，则需要指示您的开发服务器进行服务，`index.html`因为它是单页面应用程序。以下是如何使用流行的开发服务器启用此函数。

> 关于创建React应用程序的注意事项
>
> 如果您正在使用创建应用程序，则不需要配置回退URL，它会自动完成。

### 配置Express

如果您正在 Express 使用`index.html`：

```javascript
app.get('/*', (req, res) => {
  res.sendFile(path.join(__dirname, 'index.html'))
})
```

### 配置WebpackDevServer

如果您从 WebpackDevServer 中服务您的`index.html`：您可以添加到您的 webpack.config.dev.js 中：

```javascript
devServer: {
  historyApiFallback: true
}
```

## 将 React 路由器与 Redux 应用连接

在本章中，我们将使用 [Todos ](https://github.com/reactjs/redux/tree/master/examples/todos)例子。我们建议您在阅读本章时复制它。

首先，我们将需要从阵营路由器输入`<Router` `/>`和`<Route` `/>`。以下是如何做到这一点：

```javascript
import { Router, Route } from 'react-router'
```

在一个 React 应用程序中，通常当 URL 改变时，你会在`<Router` `/>`中会包装`<Route` `/>`，这样`<Router` `/>`将匹配它的一个路径分支，并且呈现它们配置的组件。`<Route` `/>`用于声明性地将路由映射到应用程序的组件层次结构。您将在`path`URL 中使用的路径中声明路由，并在`component`路由匹配 URL 时声明单个组件。

```javascript
const Root = () => (
  <Router>
    <Route path="/" component={App} />
  </Router>
)
```

但是，在我们的 Redux 应用程序中，我们仍然需要`<Provider` `/>`。`<Provider` `/>`是 React Redux 提供的高级组件，它允许您将 Redux 绑定到 React（请参阅 React 的用法）。

然后我们将从 React Redux 中导入`<Provider` `/>`：

```javascript
import { Provider } from 'react-redux'
```

我们将在`<Provider` `/>`包装`<Router` `/>`，以便路由处理程序可以访问`store`。

```javascript
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
)
```

现在，如果 URL 匹配“/” ，`<App` `/>`组件将被呈现。另外，我们将添加可选`(:filter)`参数`/`，因为当我们尝试从URL中读取参数`(:filter)`时，我们会进一步需要它。

```javascript
<Route path="/(:filter)" component={App} />
```

您可能想要从 URL 中删除散列（例如：`http://localhost:3000/#/?_k=4sbb0i`）。为此，您还需要从 React 路由器中导入`browserHistory`：

```javascript
import { Router, Route, browserHistory } from 'react-router'
```

并将其传递给`<Router` `/>`以从 URL 中删除 hash 值：

```javascript
<Router history={browserHistory}>
  <Route path="/(:filter)" component={App} />
</Router>
```

除非你的目标是像 IE9 这样的老式浏览器，否则你可以随时使用`browserHistory`。

#### `components/Root.js`

```javascript
import React from 'react'
import PropTypes from 'prop-types'
import { Provider } from 'react-redux'
import { Router, Route, browserHistory } from 'react-router'
import App from './App'

const Root = ({ store }) => (
  <Provider store={store}>
    <Router history={browserHistory}>
      <Route path="/(:filter)" component={App} />
    </Router>
  </Provider>
)

Root.propTypes = {
  store: PropTypes.object.isRequired
}

export default Root
```

我们还需要重构`index.js`将`<Root` `/>`组件呈现给 DOM。

#### `index.js`

```javascript
import React from 'react'
import { render } from 'react-dom'
import { createStore } from 'redux'
import todoApp from './reducers'
import Root from './components/Root'

let store = createStore(todoApp)

render(
  <Root store={store} />,
  document.getElementById('root')
)
```

## 使用 React 路由器进行导航

React 路由器附带一个[``](https://github.com/ReactTraining/react-router/blob/v3/docs/API.md#link)组件，可让您浏览应用程序。在我们的例子中，我们可以包装`<Link` `/>`一个新的容器组件`<FilterLink` `/>`，以便动态更改 URL。`activeStyle={}`属性让我们对活动状态应用样式。

#### `containers/FilterLink.js`

```javascript
import React from 'react'
import { Link } from 'react-router'

const FilterLink = ({ filter, children }) => (
  <Link
    to={filter === 'SHOW_ALL' ? '/' : filter}
    activeStyle={{
      textDecoration: 'none',
      color: 'black'
    }}
  >
    {children}
  </Link>
)

export default FilterLink
```

#### `components/Footer.js`

```javascript
import React from 'react'
import FilterLink from '../containers/FilterLink'

const Footer = () => (
  <p>
    Show:
    {' '}
    <FilterLink filter="SHOW_ALL">
      All
    </FilterLink>
    {', '}
    <FilterLink filter="SHOW_ACTIVE">
      Active
    </FilterLink>
    {', '}
    <FilterLink filter="SHOW_COMPLETED">
      Completed
    </FilterLink>
  </p>
)

export default Footer
```

现在，如果你点击`<FilterLink` `/>`，你会看到你的网址会之间切换`'/SHOW_COMPLETED'`，`'/SHOW_ACTIVE'`和`'/'`。即使您使用浏览器返回，它也会使用浏览器的历史记录，并有效地转到以前的网址。

## 从URL读取

目前，即使 URL 更改，待办事项列表也不会被过滤。这是因为我们正在从`<VisibleTodoList` `/>`的`mapStateToProps()`中过滤仍然是和`state`绑定的，而不是 URL。`mapStateToProps`有一个可选的第二个参数`ownProps`，该参数是传递`<VisibleTodoList` `/>`给每个道具的对象

#### `containers/VisibleTodoList.js`

```javascript
const mapStateToProps = (state, ownProps) => {
  return {
    todos: getVisibleTodos(state.todos, ownProps.filter) // previously was getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```

现在我们没有传递任何东西给`<App` `/>`所以`ownProps`是空的对象。要根据 URL 过滤我们的待办事项，我们想要传递 URL 参数给`<VisibleTodoList` `/>`。

之前我们写过：`<Route` `path="/(:filter)"` `component={App}` `/>`，它在`App`让一个`params`属性变得可用。

`params`属性是一个对象，每个参数都在 url 中指定。*例如：* *如果我们正在导航到*`*localhost:3000/SHOW_COMPLETED*`*__ ，则*`*params*`*等于*`*{ filter: 'SHOW_COMPLETED' }*`*。我们现在可以从*`*<App*` `*/>*`*__ 读取URL 。*

请注意，我们使用 [ES6 解构](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)的性质传递`params`到`<VisibleTodoList` `/>`。

#### `components/App.js`

```javascript
const App = ({ params }) => {
  return (
    <div>
      <AddTodo />
      <VisibleTodoList filter={params.filter || 'SHOW_ALL'} />
      <Footer />
    </div>
  )
}
```

## 下一步

现在您已经知道如何进行基本路由，您可以了解更多关于 [React Router API ](https://github.com/reactjs/react-router/tree/v3/docs/)的信息

> 关于其他路由库的注意事项
>
> *Redux Router*是一个实验库，它可以让你完全保留你的URL的状态到你的redux存储中。它具有与React路由器API相同的API，但与反应路由器相比，它具有更小的社区支持。
>
> *React Router Redux*在您的redux应用程序和react-router之间创建一个绑定，并使它们保持同步。如果没有此绑定，您将无法使用Time Travel倒带动作。除非您需要，否则React Router和Redux可以完全分开操作。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18