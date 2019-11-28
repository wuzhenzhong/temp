# 服务器渲染（Server Rendering）

- 贡献者1人

  

服务器端渲染最常见的用例是当用户（或搜索引擎搜寻器）首次请求我们的应用程序时处理*初始渲染*。当服务器接收到请求时，它将所需的组件呈现为 HTML 字符串，然后将其作为响应发送给客户端。从那时起，客户接管提交职责。

我们将在下面的示例中使用 React ，但可以将其他视图框架与可以在服务器上呈现的相同技术一起使用。

### Redux on the Server

在服务器渲染中使用 Redux 时，我们还必须在响应中发送应用程序的状态，以便客户端可以将其用作初始状态。这很重要，因为如果我们在生成 HTML 之前预加载任何数据，我们希望客户端也可以访问这些数据。否则，客户端生成的标记将与服务器标记不匹配，客户端将不得不再次加载数据。

要将数据发送给客户，我们需要：

- 在每个请求上创建一个全新的 Redux store 实例；

- 可选地派遣一些动作；

- 将状态从 store 中拉出；

- 然后将状态传递给客户端。

在客户端，将会创建一个新的 Redux 存储并使用服务器提供的状态进行初始化。

Redux 在服务器端的***唯一***工作是提供我们应用程序的**初始状态**。

## 配置

在下面的配方中，我们将看看如何设置服务器端渲染。我们将使用简单的 [Counter app ](https://github.com/reactjs/redux/tree/master/examples/counter)作为指导，并根据请求显示服务器如何提前呈现状态。

### 安装软件包

在这个例子中，我们将使用 [Express ](http://expressjs.com/)作为简单的 Web 服务器。我们还需要为 Redux 安装 Reac t绑定，因为它们默认不包含在 Redux 中。

```javascript
npm install --save express react-redux
```

## 服务器端

以下是我们的服务器端看起来像什么大纲。我们将使用 [app.use ](http://expressjs.com/api.html#app.use)建立一个 [Express 中间件 ](http://expressjs.com/guide/using-middleware.html)来处理进入我们服务器的所有请求。如果您不熟悉 Express 或中间件，只需知道每次服务器收到请求时都会调用 handleRender 函数。

另外，由于我们使用的是 ES6 和 JSX 语法，因此我们需要使用 [Babel ](https://babeljs.io/)进行编译（请参阅具有[Babel ](https://babeljs.io/)[的节点服务器示例](https://github.com/babel/example-node-server)）和 [React 预设](https://babeljs.io/docs/plugins/preset-react/)。

##### `server.js`

```javascript
import path from 'path'
import Express from 'express'
import React from 'react'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import counterApp from './reducers'
import App from './containers/App'

const app = Express()
const port = 3000

//Serve static files
app.use('/static', Express.static('static'))

// This is fired every time the server side receives a request
app.use(handleRender)

// We are going to fill these out in the sections to follow
function handleRender(req, res) { /* ... */ }
function renderFullPage(html, preloadedState) { /* ... */ }

app.listen(port)
```

### 处理请求

我们需要对每个请求执行的第一件事是创建一个新的 Redux store 实例。这个 store 实例的唯一目的是提供我们应用程序的初始状态。

在渲染时，我们将把`<App` `/>`我们的根组件包装在一个`<Provider>`中，以使存储可用于组件树中的所有组件，正如我们在用法与反应中看到的。

服务器端渲染的关键步骤是***在*** 我们将组件发送到客户端***之前*** 呈现组件的初始 HTML 。为此，我们使用 [ReactDOMServer.renderToString（）](https://facebook.github.io/react/docs/react-dom-server.html#rendertostring)。

然后，我们使用 Redux store 获取初始状态`store.getState()`。我们将看到这是如何传递给我们的`renderFullPage`函数。

```javascript
import { renderToString } from 'react-dom/server'

function handleRender(req, res) {
  // Create a new Redux store instance
  const store = createStore(counterApp)

  // Render the component to a string
  const html = renderToString(
    <Provider store={store}>
      <App />
    </Provider>
  )

  // Grab the initial state from our Redux store
  const preloadedState = store.getState()

  // Send the rendered page back to the client
  res.send(renderFullPage(html, preloadedState))
}
```

### 注入初始组件HTML和状态

服务器端的最后一步是将我们的初始组件HTML和初始状态注入要在客户端呈现的模板中。为了传递状态，我们添加一个`<script>`将附加`preloadedState`到的标签`window.__PRELOADED_STATE__`。

在`preloadedState`随后将通过访问可在客户端上`window.__PRELOADED_STATE__`。

我们还通过脚本标记包含了我们的客户端应用程序的包文件。这是您的捆绑工具为您的客户入口点提供的任何输出。它可能是一个静态文件或热重载开发服务器的 URL 。

```javascript
function renderFullPage(html, preloadedState) {
  return `
    <!doctype html>
    <html>
      <head>
        <title>Redux Universal Example</title>
      </head>
      <body>
        <div id="root">${html}</div>
        <script>
          // WARNING: See the following for security issues around embedding JSON in HTML:
          // http://redux.js.org/docs/recipes/ServerRendering.html#security-considerations
          window.__PRELOADED_STATE__ = ${JSON.stringify(preloadedState).replace(/</g, '\\u003c')}
        </script>
        <script src="/static/bundle.js"></script>
      </body>
    </html>
    `
}
```

## 客户端

客户端非常简单。我们所需要做的就是从初始状态获取初始状态`window.__PRELOADED_STATE__`，并将其`createStore()`作为初始状态传递给我们的函数。

我们来看看我们的新客户端文件：

#### `client.js`

```javascript
import React from 'react'
import { render } from 'react-dom'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import App from './containers/App'
import counterApp from './reducers'

// Grab the state from a global variable injected into the server-generated HTML
const preloadedState = window.__PRELOADED_STATE__

// Allow the passed state to be garbage-collected
delete window.__PRELOADED_STATE__

// Create Redux store with initial state
const store = createStore(counterApp, preloadedState)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

您可以设置您选择的构建工具（Webpack，Browserify等）来编译一个包文件`static/bundle.js`。

当页面加载时，捆绑文件将被启动，[`ReactDOM.render()`](https://facebook.github.io/react/docs/top-level-api.html#reactdom.render)并将钩入`data-react-id`服务器呈现的 HTML 中的属性。这会将我们新开始的React实例连接到服务器上使用的虚拟DOM。由于我们的 Redux 存储具有相同的初始状态，并且对所有视图组件使用相同的代码，因此结果将是相同的真实 DOM。

就是这样！这就是我们需要做的实现服务器端渲染。

但结果是相当 vanilla 。它基本上呈现了动态代码的静态视图。接下来我们需要做的是动态构建一个初始状态，以使渲染视图成为动态的。

## 准备初始状态

由于客户端执行正在进行的代码，因此它可以从空初始状态开始，随时随地获取所需的任何状态。在服务器端，渲染是同步的，我们只用一次渲染来渲染视图。我们需要能够在请求期间编译我们的初始状态，这将不得不对输入做出反应并获得外部状态（例如来自API或数据库的状态）。

### 处理请求参数

服务器端代码的唯一输入是在浏览器中加载应用程序中的页面时发出的请求。您可以选择在引导期间配置服务器（例如，当您在开发与生产环境中运行时），但该配置是静态的。

该请求包含有关请求的URL的信息，包括任何查询参数，这些参数在使用类似 [React Router ](https://github.com/reactjs/react-router)时很有用。它也可以包含带有 Cookie 或授权等输入的标题或 POST 正文数据。我们来看看如何根据查询参数设置初始计数器状态。

#### `server.js`

```javascript
import qs from 'qs' // Add this at the top of the file
import { renderToString } from 'react-dom/server'

function handleRender(req, res) {
  // Read the counter from the request, if provided
  const params = qs.parse(req.query)
  const counter = parseInt(params.counter, 10) || 0

  // Compile an initial state
  let preloadedState = { counter }

  // Create a new Redux store instance
  const store = createStore(counterApp, preloadedState)

  // Render the component to a string
  const html = renderToString(
    <Provider store={store}>
      <App />
    </Provider>
  )

  // Grab the initial state from our Redux store
  const finalState = store.getState()

  // Send the rendered page back to the client
  res.send(renderFullPage(html, finalState))
}
```

代码从`Request`传递给我们服务器中间件的 Express 对象读取。该参数被解析为一个数字，然后设置为初始状态。如果您在浏览器中访问 http://localhost:3000/?counter=100，则会看到计数器从100开始。在呈现的 HTML 中，您将看到计数器输出为 100，`__PRELOADED_STATE__`变量具有计数器设置在里面。

### 异步状态提取

服务器端渲染最常见的问题是处理异步进入的状态。在服务器上进行渲染本质上是同步的，所以有必要将任何异步提取映射到同步操作中。

最简单的方法是将一些回调传递回同步代码。在这种情况下，这将是一个函数，它将引用响应对象并将我们呈现的 HTML 发送回客户端。别担心，它听起来不那么难。

对于我们的例子，我们会想象有一个包含计数器初始值的外部数据存储区（Counter As A Service 或 CaaS）。我们会对他们进行模拟调用，并从结果中建立我们的初始状态。我们将开始构建我们的 API 调用：

#### `api/counter.js`

```javascript
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min)) + min
}

export function fetchCounter(callback) {
  setTimeout(() => {
    callback(getRandomInt(1, 100))
  }, 500)
}
```

同样，这只是一个模拟 API ，所以我们用它`setTimeout`来模拟一个需要500毫秒响应的网络请求（使用真实世界的 API 应该快得多）。我们传递一个异步返回一个随机数的回调函数。如果您使用的是基于 Promise 的 API 客户端，那么您将在您的`then`处理程序中发出此回调。

在服务器端，我们只是简单地包装我们现有的代码`fetchCounter`并在回调中接收结果：

#### `server.js`

```javascript
// Add this to our imports
import { fetchCounter } from './api/counter'
import { renderToString } from 'react-dom/server'

function handleRender(req, res) {
  // Query our mock API asynchronously
  fetchCounter(apiResult => {
    // Read the counter from the request, if provided
    const params = qs.parse(req.query)
    const counter = parseInt(params.counter, 10) || apiResult || 0

    // Compile an initial state
    let preloadedState = { counter }

    // Create a new Redux store instance
    const store = createStore(counterApp, preloadedState)

    // Render the component to a string
    const html = renderToString(
      <Provider store={store}>
        <App />
      </Provider>
    )

    // Grab the initial state from our Redux store
    const finalState = store.getState()

    // Send the rendered page back to the client
    res.send(renderFullPage(html, finalState))
  })
}
```

由于我们`res.send()`在回调中调用，服务器将保持打开连接，并且不会发送任何数据，直到该回调执行为止。您会注意到，由于我们的新 API 调用，现在每个服务器请求都会添加一个500 毫秒的延迟。更高级的用法将优雅地处理 API 中的错误，例如错误的响应或超时。

### 安全考虑

由于我们引入了更多依赖于用户生成内容（UGC）和输入的代码，因此我们增加了我们的应用程序的攻击面积。对于任何应用程序来说，确保您的输入已经过适当的消毒以防止诸如跨站脚本攻击（XSS）攻击或代码注入等情况，这一点非常重要。

在我们的例子中，我们采取了基本的安全方法。当我们获得来自请求的参数中，我们使用`parseInt`的`counter`参数，以确保此值是一个数字。如果我们不这样做，通过在请求中提供一个脚本标记，您可以轻松地将危险数据放入呈现的HTML中。这可能是这样的：`?counter=</script><script>doSomethingBad();</script>`

对于我们的简单例子，强制我们的输入成为一个数字是足够安全的。如果您正在处理更复杂的输入（例如自由格式文本），则应该通过适当的清理功能（例如[validator.js）](https://www.npmjs.com/package/validator)运行该输入。

此外，您可以通过清理状态输出来添加额外的安全层。`JSON.stringify`可以接受脚本注射。为了解决这个问题，你可以擦除JSON字符串的HTML标签和其他危险字符。这可以通过对字符串进行简单的文本替换来完成，例如`JSON.stringify(state).replace(/</g, '\\u003c')`，或者通过更复杂的库（如[serialize-javascript）来完成](https://github.com/yahoo/serialize-javascript)。

## 下一步

您可能需要阅读异步操作以了解有关使用异步基元（如 Promise 和 thunk ）在 Redux 中表达异步流的更多信息。请记住，您在那里学到的任何东西都可以应用于通用渲染。

如果你使用了[React Router 之](https://github.com/reactjs/react-router)类的东西，你可能也想要`fetchData()`在你的路由处理器组件中将你的数据获取依赖关系表示为静态方法。他们可能会返回异步动作，以便您的`handleRender`函数可以将路由匹配到路由处理程序组件类，`fetchData() `为每个类分派结果，并且仅在 Promises 解析后呈现。通过这种方式，不同路由所需的特定API调用与路由处理程序组件定义共同驻留。您也可以在客户端使用相同的技术，以防止路由器切换页面直到其数据被加载。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18