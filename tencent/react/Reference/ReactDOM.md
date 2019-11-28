# ReactDOM

如果您从`<script>`标记加载React ，则这些顶级API可在`ReactDOM`全局中使用。如果你使用npm的ES6，你可以写`import ReactDOM from 'react-dom'`。如果你使用npm的ES5，你可以写`var ReactDOM = require('react-dom')`。

## 概观

[纠错](javascript:;)

该`react-dom`软件包提供了DOM特定的方法，可以在应用程序的顶层使用，也可以作为退出孵化器，以便在需要时脱离React模型。大多数组件不应该需要使用此模块。

- `render()`

- `hydrate()`

- `unmountComponentAtNode()`

- `findDOMNode()`

- `createPortal()`

### 浏览器支持

React支持所有流行的浏览器，包括Internet Explorer 9及以上版本。

> 注意我们不支持不支持ES5方法的旧浏览器，但如果页面中包含[es5-shim和es5-sham](https://github.com/es-shims/es5-shim)等[填充，](https://github.com/es-shims/es5-shim)则可能会发现您的应用在较旧的浏览器中可以正常工作。如果你选择走这条道路，那么你是独立的。

## 参考

### `render()`

```javascript
ReactDOM.render(element, container[, callback])
```

将React元素渲染到提供的DOM中，`container`并返回对组件的引用（或返回`null`无状态组件）。

如果先前已将React元素渲染到`container`该元素中，则会对其执行更新，并根据需要仅更改DOM以反映最新的React元素。

如果提供了可选的回调，它将在组件被渲染或更新后执行。

> 注意： `ReactDOM.render()`控制传入的容器节点的内容。当第一次调用时，内部的任何现有DOM元素都将被替换。后来的调用使用React的DOM差异算法进行高效更新。 `ReactDOM.render()`不修改容器节点（只修改容器的子节点）。将组件插入现有的DOM节点而不覆盖现有的子组件可能是可能的。 `ReactDOM.render()`当前返回对根`ReactComponent`实例的引用。但是，使用此返回值是遗留的，应该避免，因为在某些情况下，未来版本的React可能会异步渲染组件。如果您需要对根`ReactComponent`实例的引用，则首选解决方案是将回调引用附加到根元素。运用`ReactDOM.render()`保证服务器渲染的容器已被弃用，并将在React 17中被移除`hydrate()`。

### `hydrate()`

```javascript
ReactDOM.hydrate(element, container[, callback])
```

与之相同`render()`，但用于为其HTML内容由其呈现的容器提供水合`ReactDOMServer`。React将尝试将事件监听器附加到现有的标记。

React期望服务器和客户端之间呈现的内容是相同的。它可以修补文本内容的不同（例如时间戳），但您应该将不匹配视为错误并修复它们。在开发模式中，React在水合期间警告不匹配。不能保证在不匹配的情况下修改属性差异。这对于性能方面的原因很重要，因为在大多数应用程序中，不匹配是很少见的，因此验证所有标记将会极其昂贵。

如果您故意需要在服务器和客户端上渲染不同的东西，则可以执行两遍渲染。在客户端上渲染不同内容的组件可以读取一个状态变量`this.state.isClient`，如您可以设置的`true`那样`componentDidMount()`。通过这种方式，初始渲染过程将呈现与服务器相同的内容，从而避免不匹配，但是在水合之后还会同步发生额外的传递。请注意，这种方法会让组件变得更慢，因为它们必须渲染两次，所以请谨慎使用。

请记住注意慢速连接的用户体验。JavaScript代码的加载时间可能比初始HTML呈现时间要晚得多，所以如果您在仅客户端传递中呈现不同的内容，则转换可能会令人震惊。但是，如果执行得当，在服务器上渲染应用程序的“shell”可能是有益的，并且仅显示客户端上的一些额外小部件。要了解如何在不出现标记不匹配问题的情况下执行此操作，请参阅前一段中的解释。

### `unmountComponentAtNode()`

```javascript
ReactDOM.unmountComponentAtNode(container)
```

从DOM中删除已安装的React组件并清理其事件处理程序和状态。如果容器中没有安装组件，则调用此函数不会执行任何操作。返回`true`组件是否卸载以及`false`是否没有组件卸载。

### `findDOMNode()`

```javascript
ReactDOM.findDOMNode(component)
```

如果此组件已装入DOM，则会返回相应的本地浏览器DOM元素。此方法对于从DOM中读取值（如表单域值和执行DOM测量）非常有用。**在大多数情况下，您可以将一个ref附加到DOM节点，并避免使用findDOMNode。**

当组件呈现到`null`或`false`，`findDOMNode`返回`null`。当组件呈现为字符串时，`findDOMNode`返回包含该值的文本DOM节点。从React 16开始，组件可能会返回一个带有多个子元素的片段，在这种情况下`findDOMNode`将返回与第一个非空子元素相对应的DOM节点。

> 注意： `findDOMNode`是一个用于访问底层DOM节点的逃生舱口。在大多数情况下，不鼓励使用这个逃生舱口，因为它穿透了组件抽象。 `findDOMNode`仅适用于安装的组件（即已放置在DOM中的组件）。如果你试图调用此说尚未安装（如调用组件`findDOMNode()`中`render()`，目前尚未将要创建的组件）的异常将被抛出。 `findDOMNode`不能用于功能组件。

### `createPortal()`

```javascript
ReactDOM.createPortal(child, container)
```

创建一个门户。门户提供了一种将子元素呈现到DOM组件层次结构之外的DOM节点的方法。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com