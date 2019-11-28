# State and Lifecycle

考虑上一节中的滴答时钟示例。

到目前为止，我们只学习了一种更新 UI 的方法。

我们调用`ReactDOM.render()`改变呈现的输出：

```javascript
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

**在 CodePen 上试用它。**

在本节中，我们将学习如何使`Clock`组件真正可重用和封装。它会设置自己的计时器并每秒更新一次。

我们可以从封装时钟的外观开始：

```javascript
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

**在 CodePen 上试用它。**

但是，它忽略了一个关键要求：`Clock`设置计时器并每秒更新UI 的事实应该是该实现的实现细节`Clock`。

理想情况下，我们想写一次，并有`Clock`更新本身：

```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

为了实现这个，我们需要给组件添加“状态” `Clock`。

状态类似于道具，但是它是私人的并且完全由组件控制。

我们之前提到，定义为类的组件具有一些附加功能。本地状态就是这样：一个仅适用于类的功能。

## 将函数转换为类

您可以通过`Clock`五个步骤将功能组件转换为类：

1. 创建一个扩展名为 **ES6 的类**，名称相同`React.Component`。

1. 为它添加一个空的方法`render()`。

1. 将函数的主体移到`render()`方法中。

1. 更换`props`用`this.props`的`render()`身体。

1. 删除剩余的空函数declaration.class Clock extends React.Component {render（）{return（<div> <h1> Hello，world！</ h1> <h2> {this.props.date.toLocaleTimeString（）} 。</ h2> </ div>）; }} **在 CodePen 上试试它。**`Clock`现在被定义为一个类而不是一个函数。这让我们可以使用额外的功能，例如本地状态和生命周期钩子。将本地状态添加到类我们将通过`date`三个步骤将从道具移动到状态：

1. 在该方法中替换`this.props.date`为：`this.state.daterender()`

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

\1. 添加一个**类的构造函数**，指定初始值 `this.state`:class Clock extends React.Component { constructor(props) { super(props); this.state = {date: new Date()}; } render() { return ( <div> <h1>Hello, world!</h1> <h2>It is {this.state.date.toLocaleTimeString()}.</h2> </div> ); } }Note how we pass `props` to the base constructor: constructor(props) { super(props); this.state = {date: new Date()}; }Class components should always call the base constructor with `props`.

1. Remove the `date` prop from the `<Clock />` element:

```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

稍后我们将计时器代码添加回组件本身。

结果如下所示：

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

接下来，我们将`Clock`设置自己的计时器并每秒更新一次。

## 将生命周期方法添加到类

在具有多个组件的应用程序中，释放组件在销毁时所占用的资源非常重要。

我们想要在第一次呈现给DOM 时**设置一个计时器**`Clock`。这在React中被称为“挂载”。

我们也想**清除该定时器**[，](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval)只要`Clock`删除由该DOM生成的DOM 。这在React中被称为“卸载”。

我们可以在组件类上声明特殊的方法来在组件装载和卸载时运行一些代码：

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

这些方法被称为“生命周期挂钩”。

在将组件输出呈现给 DOM 后，`componentDidMount()`在将组件输出呈现给 DOM 后，该钩子运行。这是设置计时器的好地方：

```javascript
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

注意我们如何保存定时器 ID `this`。

虽然`this.props`由 React 自己设置并`this.state`具有特殊含义，但如果您需要存储某些不用于视觉输出的内容，则可以自由向该类手动添加其他字段。

如果你不使用某些东西`render()`，它不应该处于这种状态。

我们将拆除`componentWillUnmount()`生命周期钩子中的计时器：

```javascript
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

最后，我们将实现一个调用的方法`tick()`，`Clock`组件将运行每一秒。

它将`this.setState()`用于安排组件本地状态的更新：

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

现在时钟每秒钟都在滴答。

让我们快速回顾发生了什么以及调用方法的顺序：

1. 当`<Clock />`传递给`ReactDOM.render()`React 时，React 调用`Clock`组件的构造函数。由于`Clock`需要显示当前时间，因此它会`this.state`使用包含当前时间的对象进行初始化。我们稍后将更新这个状态。

1. React 然后调用`Clock`组件的`render()`方法。这就是 React 如何学习屏幕上应显示的内容。React 然后更新 DOM 以匹配`Clock`渲染输出。

1. 当`Clock`输出插入到 DOM 中时，React调用`componentDidMount()`生命周期钩子。在它里面，`Clock`组件要求浏览器设置一个计时器，`tick()`每秒调用一次该组件的方法。

1. 浏览器每秒调用一次该`tick()`方法。在它内部，`Clock`组件通过调用`setState()`包含当前时间的对象来调度 UI 更新。感谢`setState()`电话，React 知道状态已经改变，并`render()`再次调用方法来了解屏幕上应显示的内容。这一次，`this.state.date`该`render()`方法将会不同，因此渲染输出将包含更新的时间。React 会相应地更新 DOM。

1. 如果`Clock`组件从 DOM 中移除，React 将调用`componentWillUnmount()`生命周期钩子，以便定时器停止。

## 正确使用状态

有三件事你应该知道`setState()`。

### 不要直接修改状态

例如，这不会重新渲染组件：

```javascript
// Wrong
this.state.comment = 'Hello';
```

相反，使用`setState()`：

```javascript
// Correct
this.setState({comment: 'Hello'});
```

唯一可以分配的地方`this.state`是构造函数。

### 状态更新可能是异步的

React 可能会将多个`setState()`调用分批到单个更新中进行性能调优。

因为`this.props`和`this.state`可异步更新，你不应该依赖于它们的值来计算下一个状态。

例如，此代码可能无法更新计数器：

```javascript
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

要修复它，请使用`setState()`接受函数而不是对象的第二种形式。该函数将接收前一个状态作为第一个参数，并将更新应用时的道具作为第二个参数：

```javascript
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

我们使用了上面的**箭头函数**，但它也适用于常规函数：

```javascript
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### 状态更新已合并

当你打电话时`setState()`，React 将你提供的对象合并到当前状态。

例如，您的状态可能包含多个独立变量：

```javascript
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

然后，您可以使用单独的`setState()`调用独立更新它们：

```javascript
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

合并很浅，所以`this.setState({comments})`叶子`this.state.posts`完好无损，但完全取代`this.state.comments`。

## 数据流向下

父母和孩子的组件都不知道某个组件是有状态的还是无状态的，它们不应该关心它是被定义为一个函数还是一个类。

这就是为什么状态通常被称为本地或封装。除了拥有和设置它的组件之外，其他任何组件都无法访问它。

组件可以选择将其状态作为道具传递给其子组件：

```javascript
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

这也适用于用户定义的组件：

```javascript
<FormattedDate date={this.state.date} />
```

`FormattedDate`组件会收到`date`它的道具，并不知道它是来自`Clock`国家，`Clock`道具还是手工输入：

```javascript
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

**在 CodePen 上试用它。**

这通常称为“自顶向下”或“单向”数据流。任何状态总是由某个特定组件拥有，并且从该状态派生的任何数据或 UI 只能影响树中“在其下”的组件。

如果将组件树想象成道具的瀑布，则每个组件的状态就像是一个额外的水源，它可以在任意点加入它，但也会流下来。

为了显示所有组件都是真正隔离的，我们可以创建一个`App`呈现三个`<Clock>`的组件：

```javascript
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

每个`Clock`设置自己的计时器并独立更新。

在 React 应用程序中，无论组件是有状态的还是无状态的，都被视为可能随时间而改变的组件的实现细节。您可以在有状态组件内使用无状态组件，反之亦然。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com