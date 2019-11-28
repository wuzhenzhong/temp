# 示例（Examples）

- 贡献者1人

  

Redux 在其[源代码中](https://github.com/reactjs/redux/tree/master/examples)分发了几个例子。这些例子中的大部分也都在 [CodeSandbox 上](https://codesandbox.io/)，这是一个在线编辑器，可让您在线[演示](https://codesandbox.io/)示例。

## Counter Vanilla

运行 [Counter Vanilla ](https://github.com/reactjs/redux/tree/master/examples/counter-vanilla)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/counter-vanilla
open index.html
```

它不需要构建系统或视图框架，并且存在以显示用于 ES5 的原始 Redux API。

## Counter

运行 [Counter ](https://github.com/reactjs/redux/tree/master/examples/counter)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/counter
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/counter)。

这是与 React 一起使用 Redux 的最基本的例子。为了简单起见，当 store 更改时，它会手动重新呈现 React 组件。在真实项目中，您可能想要使用高性能的 [React Redux ](https://github.com/reactjs/react-redux)绑定。

这个例子包括测试。

## Todos

运行 [Todos ](https://github.com/reactjs/redux/tree/master/examples/todos)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/todos
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/todos)。

这是更好地了解状态更新如何与 Redux 中的组件一起工作的最佳示例。它显示了 reducer 如何将处理操作委托给其他 reducer ，以及如何使用 [React Redux ](https://github.com/reactjs/react-redux)从您的演示组件中生成容器组件。

这个示例包括测试。

## Todos with Undo

[使用撤销](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)示例运行 [Todos ](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/todos-with-undo
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/todos-with-undo)。

这是前一个示例的变化。它几乎完全相同，但是还显示了如何用 [Redux Undo ](https://github.com/omnidan/redux-undo)包装 Reducer 以便使用几行代码为应用程序添加撤销/重做功能。

## TodoMVC

运行 [TodoMVC ](https://github.com/reactjs/redux/tree/master/examples/todomvc)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/todomvc
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/todomvc)。

这是经典的 [TodoMVC ](http://todomvc.com/)示例。它仅仅是为了比较，但它涵盖了与 Todos 例子相同的点。

这个例子包括测试。

## Shopping Cart

运行 [Shopping Cart ](https://github.com/reactjs/redux/tree/master/examples/shopping-cart)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/shopping-cart
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox ](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/shopping-cart)。

这个例子显示了随着你的应用程序的增长，重要的惯用 Redux 模式变得重要。特别是，它展示了如何通过标识符以规范化的方式存储实体，如何在几个层次上组合缩减器，以及如何在缩减器旁边定义选择器，以便封装状态形状的知识。它还演示使用 [Redux Logger 进行](https://github.com/fcomb/redux-logger)日志[记录](https://github.com/fcomb/redux-logger)并使用 [Redux Thunk ](https://github.com/gaearon/redux-thunk)中间件有条件地分配操作。

## Tree View

运行 [Tree View ](https://github.com/reactjs/redux/tree/master/examples/tree-view)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/tree-view
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox ](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/tree-view)。

这个例子演示了一个深度嵌套的树视图，并以规范化的形式表示它的状态，所以很容易从reducer中更新。良好的渲染性能是通过容器组件精细地订阅它们呈现的树节点来实现的。

此例子包括测试。

## Async

运行 [Async ](https://github.com/reactjs/redux/tree/master/examples/async)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/async
npm install
npm start

open http://localhost:3000/
```

或者查看 [sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/async)。

此示例包括从异步 API 读取，响应用户输入提取数据，显示加载指示符，缓存响应以及使缓存无效。它使用 [Redux Thunk ](https://github.com/gaearon/redux-thunk)中间件来封装异步 side effects。

## Universal

运行 [Universal ](https://github.com/reactjs/redux/tree/master/examples/universal)示例：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/universal
npm install
npm start

open http://localhost:3000/
```

这是使用 Redux 和 React 进行服务器渲染的基本演示。它显示了如何准备服务器上的初始存储状态，并将其传递给客户端，以便客户端存储可以从现有状态启动。

## Real World

运行 [Real World ](https://github.com/reactjs/redux/tree/master/examples/real-world)例子：

```javascript
git clone https://github.com/reactjs/redux.git

cd redux/examples/real-world
npm install
npm start

open http://localhost:3000/
```

或者查看[sandbox](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/real-world)。

这是最先进的例子。它是由设计密集。它包括将获取的实体保存在规范化缓存中，为API调用实现定制中间件，渲染部分加载的数据，分页，缓存响应，显示错误消息和路由。此外，它还包含 Redux DevTools。

## 更多示例

你可以在 [Awesome Redux 中](https://github.com/xgrommx/awesome-redux)找到更多的例子。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18