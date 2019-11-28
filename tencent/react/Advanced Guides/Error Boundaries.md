# Error Boundaries

过去，组件内部的JavaScript错误用于破坏React的内部状态并导致它在下次呈现时[发出](https://github.com/facebook/react/issues/4026) [隐蔽](https://github.com/facebook/react/issues/6895) [错误](https://github.com/facebook/react/issues/8579)。这些错误总是由应用程序代码中的早期错误引起的，但React没有提供在组件中正常处理它们的方法，并且无法从它们中恢复。

## 介绍错误边界

部分UI中的JavaScript错误不应该破坏整个应用程序。为了解决React用户的这个问题，React 16引入了一个“错误边界”的新概念。

错误边界是React组件，**可以在其子组件树中的任何位置捕获JavaScript错误，记录这些错误并显示回退UI，**而不是崩溃的组件树。错误边界在渲染期间，生命周期方法以及整个树下的构造函数中捕获错误。

> 注意错误边界**不会**捕获以下错误：

- 事件处理程序（了解更多）

- 异步代码（例如`setTimeout`或`requestAnimationFrame`回调）

- 服务器端渲染

- 错误边界本身（而不是它的子项）抛出的错误

如果一个类组件定义了一个新的生命周期方法，它将成为一个错误边界`componentDidCatch(error, info)`：

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

然后，您可以将其用作常规组件：

```javascript
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

该`componentDidCatch()`方法像JavaScript `catch {}`块一样工作，但对于组件。只有类组件可能是错误边界。在实践中，大多数时候您会想要声明一个错误边界组件并在整个应用程序中使用它。

请注意，**错误边界只会在树中的下面的组件中捕获错误**。错误边界本身不能捕获错误。如果错误边界尝试呈现错误消息失败，则错误将传播到其上方最接近的错误边界。这也与catch {}块在JavaScript中的工作方式类似。

### componentDidCatch Parameters

`error` 是一个已经抛出的错误。

`info`是一个`componentStack`关键的对象。在抛出错误期间，该属性具有关于组件堆栈的信息。

```javascript
//...
componentDidCatch(error, info) {
  
  /* Example stack information:
     in ComponentThatThrows (created by App)
     in ErrorBoundary (created by App)
     in div (created by App)
     in App
  */
  logComponentStackToMyService(info.componentStack);
}

//...
```

## 现场演示

[声明和使用错误的边界这个例子](https://codepen.io/gaearon/pen/wqvxGa?editors=0010)与[阵营16测试版](https://github.com/facebook/react/issues/10294)。

## 何处放置错误边界

错误界限的粒度取决于您。您可能会封装顶级路由组件以向用户显示“出错了”消息，就像服务器端框架经常处理崩溃一样。您也可以将各个小部件封装在错误边界内，以防止其崩溃应用程序的其余部分。

## 未捕获错误的新行为

这一变化具有重要意义。**从React 16开始，没有被任何错误边界捕获的错误将导致整个React组件树的卸载。**

我们辩论了这个决定，但根据我们的经验，离开损坏的用户界面比彻底删除它更糟糕。例如，像Messenger这样的产品将可见的UI留下，可能会导致某人向错误的人发送消息。同样，支付应用程序显示错误的数量比不呈现任何内容更糟糕。

这一变化意味着，当您迁移到React 16时，您可能会发现以前未被注意到的应用程序中现有的崩溃。当出现错误时，添加错误边界可以让您提供更好的用户体验。

例如，Facebook Messenger将边栏，信息面板，会话日志和消息输入内容封装在单独的错误边界中。如果其中一个UI区域中的某个组件发生崩溃，则其余组件保持互动。

我们还鼓励您使用JS错误报告服务（或自己构建），以便您可以了解在生产中发生的未处理异常并对其进行修复。

## 组件栈跟踪

即使应用程序意外吞下它们，React 16也会将所有在渲染过程中发生的错误打印到开发中的控制台。除了错误消息和JavaScript堆栈之外，它还提供组件堆栈跟踪。现在您可以看到组件树中确切发生故障的位置：

![img](https://ask.qcloudimg.com/http-save/devdocs/4rbtzfahiu.png)

您还可以在组件堆栈跟踪中看到文件名和行号。这在默认情况下在[Create React App](https://github.com/facebookincubator/create-react-app)项目中起作用：

![img](https://ask.qcloudimg.com/http-save/devdocs/mmh1pnf3xg.png)

如果您不使用Create React App，则可以手动[将此插件](https://www.npmjs.com/package/babel-plugin-transform-react-jsx-source)添加到您的Babel配置中。请注意，它仅用于开发，并且**必须在生产中禁用**。

> 注意堆栈跟踪中显示的组件名称取决于[`Function.name`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name)属性。如果您支持旧版浏览器和尚未提供此功能的设备（例如IE 11），请考虑`Function.name`在您的捆绑应用程序中包含一个polyfill，例如[`function.name-polyfill`](https://github.com/JamesMGreene/Function.name)。或者，您可以`displayName`在所有组件上明确设置属性。

## 如何尝试/抓住？

`try`/ `catch`很好，但它只适用于命令式代码：

```javascript
try {
  showButton();
} catch (error) {
  // ...
}
```

然而，反应的组分是声明，并指定*哪些*应该呈现：

```javascript
<Button />
```

错误边界保留了React的声明性质，并按照您的预期行事。例如，即使树中深处`componentDidUpdate`的`setState`某个钩子发生错误，它仍然会正确传播到最近的错误边界。

## 如何处理事件处理程序？

错误边界**不会**在事件处理程序中捕获错误。

React不需要错误边界从事件处理程序中的错误中恢复。与渲染方法和生命周期钩子不同，事件处理程序在渲染过程中不会发生。所以如果他们抛出，React仍然知道要在屏幕上显示什么。

如果您需要在事件处理程序中捕获错误，请使用常规的JavaScript `try`/ `catch`语句：

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { error: null };
  }
  
  handleClick = () => {
    try {
      // Do something that could throw
    } catch (error) {
      this.setState({ error });
    }
  }

  render() {
    if (this.state.error) {
      return <h1>Caught an error.</h1>
    }
    return <div onClick={this.handleClick}>Click Me</div>
  }
}
```

请注意，上面的示例演示了常规的JavaScript行为并且不使用错误边界。

## 从React命名更改15

React 15在一个不同的方法名称下包含了对错误边界的非常有限的支持：`unstable_handleError`。此方法不再有效，您需要`componentDidCatch`从第16个beta版本开始将其更改为代码。

对于这个改变，我们提供了一个[codemod](https://github.com/reactjs/react-codemod#error-boundaries)来自动迁移你的代码。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18