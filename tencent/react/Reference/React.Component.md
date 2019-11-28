# React.Component

组件让你可以将用户界面分成独立的，可重复使用的部分，并且可以独立思考每一部分。`React.Component`由...提供`React`。

## 概观

`React.Component`是一个抽象的基类，因此`React.Component`直接引用它很少有意义。相反，您通常会对其进行子类化，并至少定义一种`render()`方法。

通常你会将一个React组件定义为一个普通的[JavaScript类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

如果您还没有使用ES6，则可以使用`create-react-class`模块。看看使用没有ES6的React来了解更多。

### 组件生命周期

每个组件都有几个“生命周期方法”，您可以重写以在该进程中的特定时间运行代码。**will**在事件发生之前调用带有前缀的方法，并且**did**在事件发生之后调用带有前缀的方法。

#### 安装

当创建一个组件的实例并将其插入到DOM中时，将调用这些方法：

- `constructor()`

- `componentWillMount()`

- `render()`

- `componentDidMount()`

#### 更新

更新可能由道具或状态的更改引起。当一个组件被重新渲染时，这些方法被调用：

- `componentWillReceiveProps()`

- `shouldComponentUpdate()`

- `componentWillUpdate()`

- `render()`

- `componentDidUpdate()`

#### 卸载

从DOM中删除组件时调用此方法：

- `componentWillUnmount()`错误处理当渲染期间，生命周期方法或任何子组件的构造函数中出现错误时，将调用此方法。

- `componentDidCatch()`

### 其他API

每个组件还提供一些其他API：

- `setState()`

- `forceUpdate()`

### 类属性

- `defaultProps`

- `displayName`

### 实例属性

- `props`

- `state`

## 参考

### `render()`

```javascript
render()
```

`render()`方法是必需的。

被调用时，它应该检查`this.props`并`this.state`返回以下类型之一：

- **React元素。**通常通过JSX创建。元素可以是本地DOM组件（`<div />`）的表示，也可以是用户定义的组合组件（`<MyComponent />`）。

- **字符串和数字。**这些呈现为DOM中的文本节点。

- **门户网站**。创建于`ReactDOM.createPortal`。

- `null`。不渲染任何东西。

- **布尔人**。什么都不渲染。（主要存在支持`return test && <Child />`模式，其中`test`是布尔值。）

当返回`null`或`false`，`ReactDOM.findDOMNode(this)`将返回`null`。

`render()`函数应该是纯粹的，这意味着它不会修改组件状态，它每次调用时都返回相同的结果，并且它不直接与浏览器交互。如果您需要与浏览器交互，请`componentDidMount()`改为使用其他生命周期方法。保持`render()`纯粹使组件更易于思考。

> `render()`如果`shouldComponentUpdate()`返回false，则不会调用注释 。

#### 片段

您也可以`render()`使用数组返回多个项目：

```javascript
render() {
  return [
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```

> 注意：不要忘记将键添加到片段中的元素以避免重要警告。

### `constructor()`

```javascript
constructor(props)
```

React组件的构造函数在挂载之前被调用。在实现`React.Component`子类的构造函数时，您应该`super(props)`在任何其他语句之前调用。否则，`this.props`将在构造函数中定义，这可能会导致错误。

避免在构造函数中引入任何副作用或订阅。对于这些用例，请`componentDidMount()`改为使用。

构造函数是初始化状态的正确位置。为此，只需分配一个对象`this.state`; 不要尝试`setState()`从构造函数中调用。构造函数也经常用于将事件处理程序绑定到类实例。

如果您没有初始化状态并且没有绑定方法，则不需要为您的React组件实现构造函数。

在极少数情况下，可以根据道具初始化状态。这有效地“分叉”道具，并用最初的道具设置状态。下面是一个有效的`React.Component`子类构造函数的例子：

```javascript
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```

注意这种模式，因为国家不会更新任何道具。不是将道具同步到状态，而是经常想要提升状态。

如果你通过将它们用于状态来“分叉”道具，那么你可能还想实施`componentWillReceiveProps(nextProps)`以保持状态与它们保持同步。但提升状态通常更容易，并且更少出现错误。

### `componentWillMount()`

```javascript
componentWillMount()
```

`componentWillMount()`在安装发生之前立即被调用。它之前被调用`render()`，因此`setState()`在此方法中同步调用不会触发额外的渲染。一般来说，我们建议使用`constructor()`。

避免在此方法中引入任何副作用或订阅。对于这些用例，请`componentDidMount()`改为使用。

这是在服务器渲染上调用的唯一生命周期钩子。

### `componentDidMount()`

```javascript
componentDidMount()
```

`componentDidMount()`在组件被安装后立即被调用。需要DOM节点的初始化应该放在这里。如果您需要从远程端点加载数据，则这是一个实例化网络请求的好地方。

此方法是设置任何订阅的好地方。如果你这样做，不要忘记退订`componentWillUnmount()`。

调用`setState()`此方法将触发额外的渲染，但保证在同一个时间间隔内刷新。这保证即使`render()`在这种情况下将被调用两次，用户也不会看到中间状态。请谨慎使用此模式，因为它经常会导致性能问题。然而，当你需要在渲染依赖于它的大小或位置的东西之前测量DOM节点时，它可能需要类似于模态和工具提示的情况。

### `componentWillReceiveProps()`

```javascript
componentWillReceiveProps(nextProps)
```

`componentWillReceiveProps()`在挂载组件接收新道具之前被调用。如果您需要在响应更新状态托变化（例如，复位），你可以比较`this.props`和`nextProps`和执行使用状态转换`this.setState()`在此方法。

请注意，即使道具尚未更改，React也可能会调用此方法，因此如果您只想处理更改，请确保比较当前值和下一个值。当父组件重新呈现组件时，可能会发生这种情况。

`componentWillReceiveProps()`在安装过程中，React不会使用初始道具进行调用。如果组件的一些道具可能更新，它只会调用这个方法。调用`this.setState()`通常不会触发`componentWillReceiveProps()`。

### `shouldComponentUpdate()`

```javascript
shouldComponentUpdate(nextProps, nextState)
```

使用`shouldComponentUpdate()`让阵营知道，如果一个组件的输出不会受到州或道具的电流变化。默认行为是在每次状态更改时重新呈现，在绝大多数情况下，您应该依赖默认行为。

`shouldComponentUpdate()`在接收新道具或状态时在渲染前被调用。默认为`true`。此方法不用于初始渲染或何时`forceUpdate()`使用。

返回`false`不会阻止子组件在*其*状态更改时重新呈现。

目前，如果`shouldComponentUpdate()`回报`false`，然后`componentWillUpdate()`，`render()`和`componentDidUpdate()`将不会被调用。请注意，将来React可能会被`shouldComponentUpdate()`视为提示而不是严格的指令，并且返回`false`可能仍会导致组件的重新呈现。

如果在分析后确定某个特定组件速度较慢，则可以将其更改为从具有较浅属性和状态比较的`React.PureComponent`哪些实现继承`shouldComponentUpdate()`。如果你有信心，你想手工地写出来，你可以比较`this.props`与`nextProps`和`this.state`使用`nextState`，并返回`false`来告诉阵营的更新可以跳过。

我们不建议进行深度平等检查或使用`JSON.stringify()`英寸`shouldComponentUpdate()`。这是非常低效的，会损害性能。

### `componentWillUpdate()`

```javascript
componentWillUpdate(nextProps, nextState)
```

`componentWillUpdate()`在接收新道具或状态时立即被调用。使用此作为更新发生之前执行准备的机会。此方法不用于初始渲染。

请注意，你不能`this.setState()`在这里打电话; 你也不应该做任何其他事情（比如派遣一个Redux动作），它会在`componentWillUpdate()`返回之前触发一个React组件的更新。

如果您需要更新`state`以响应`props`更改，请`componentWillReceiveProps()`改用它。

> 注释
>
> `componentWillUpdate()`如果`shouldComponentUpdate()`返回false，则不会调用 。

### `componentDidUpdate()`

```javascript
componentDidUpdate(prevProps, prevState)
```

`componentDidUpdate()`在更新发生后立即调用。此方法不用于初始渲染。

在更新组件时，将此用作在DOM上操作的机会。只要您将当前的道具与以前的道具进行比较（例如，如果道具没有改变，则可能不需要网络请求），这也是做网络请求的好地方。

> 注释
>
> `componentDidUpdate()`如果`shouldComponentUpdate()`返回false，则不会调用 。

### `componentWillUnmount()`

```javascript
componentWillUnmount()
```

`componentWillUnmount()`在组件被卸载并销毁之前立即被调用。在此方法中执行任何必要的清理操作，例如使定时器失效，取消网络请求或清理在其中创建的任何订阅`componentDidMount()`。

### `componentDidCatch()`

```javascript
componentDidCatch(error, info)
```

错误边界是React组件，可以在其子组件树中的任何位置捕获JavaScript错误，记录这些错误并显示回退UI，而不是崩溃的组件树。错误边界在渲染期间，生命周期方法以及整个树下的构造函数中捕获错误。

如果类组件定义了此生命周期方法，则它将成为错误边界。调用`setState()`它可以让您在下面的树中捕获未处理的JavaScript错误并显示回退UI。只能使用错误边界从意外异常中恢复; 不要试图将它们用于控制流程。

有关更多详细信息，请参阅[*React 16中的错误处理*](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)。

> 注意错误边界只会捕获树中**下面**组件**中的**错误。错误边界本身不能捕获错误。

### `setState()`

```javascript
setState(updater[, callback])
```

`setState()`将更改排入组件状态，并告诉React该组件及其子组件需要使用更新的状态重新呈现。这是您用来更新用户界面以响应事件处理程序和服务器响应的主要方法。

将其`setState()`看作是一个*请求，*而不是一个立即的命令来更新组件。为了获得更好的感知性能，React可能会延迟它，然后一次更新几个组件。React不保证立即应用状态更改。

`setState()`并不总是立即更新组件。它可能会批处理或推迟更新，直到稍后。这可以`this.state`在调用`setState()`潜在的陷阱之后立即阅读。相反，使用`componentDidUpdate`或`setState`回调（`setState(updater, callback)`），其中任何一个保证在应用更新后触发。如果您需要根据以前的状态设置状态，请阅读`updater`下面的参数。

`setState()`将永远导致重新呈现，除非`shouldComponentUpdate()`得到回报`false`。如果正在使用可变对象并且无法实现条件呈现逻辑`shouldComponentUpdate()`，则`setState()`仅在新状态与先前状态不同时调用才能避免不必要的重新呈现。

第一个参数是`updater`带签名的函数：

```javascript
(prevState, props) => stateChange
```

`prevState`是对之前状态的参考。它不应该直接变异。相反，应通过基于来自`prevState`和的输入构建新对象来表示更改`props`。例如，假设我们想通过以下方式增加一个状态值`props.step`：

```javascript
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```

二者`prevState`并`props`通过更新功能接收保证是向上的更新。更新器的输出与浅层合并`prevState`。

第二个参数`setState()`是一个可执行的回调函数，将被执行一次`setState`完成并且组件被重新渲染。通常我们推荐使用`componentDidUpdate()`这种逻辑代替。

你可以选择传递一个对象作为第一个参数来`setState()`代替函数：

```javascript
setState(stateChange[, callback])
```

这会执行`stateChange`到新状态的浅层合并，例如调整购物车项目数量：

```javascript
this.setState({quantity: 2})
```

这种形式`setState()`也是异步的，并且在同一个周期内的多个调用可以一起进行批处理。例如，如果您尝试在同一周期内多次增加物料数量，则会导致等同于：

```javascript
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```

随后的调用将覆盖同一周期内先前调用的值，因此数量只会增加一次。如果下一个状态取决于之前的状态，我们建议使用updater函数形式，而不是：

```javascript
this.setState((prevState) => {
  return {counter: prevState.quantity + 1};
});
```

有关更多详细信息，请参阅州和生命周期指南。

### `forceUpdate()`

```javascript
component.forceUpdate(callback)
```

默认情况下，当你的组件的状态或道具改变时，你的组件将重新渲染。如果你的`render()`方法依赖于其他一些数据，你可以通过调用告诉React组件需要重新渲染`forceUpdate()`。

调用`forceUpdate()`将导致`render()`在组件上调用，跳过`shouldComponentUpdate()`。这将触发子组件的正常生命周期方法，包括`shouldComponentUpdate()`每个孩子的方法。如果标记更改，React仍然只会更新DOM。

通常情况下，你应该尽量避免的所有使用`forceUpdate()`和只读`this.props`，并`this.state`在`render()`。

## 类属性

### `defaultProps`

`defaultProps`可以被定义为组件类本身的属性，以设置该类的默认道具。这用于未定义的道具，但不适用于null道具。例如：

```javascript
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
```

如果`props.color`没有提供，它将被默认设置为`'blue'`：

```javascript
  render() {
    return <CustomButton /> ; // props.color will be set to blue
  }
```

如果`props.color`设置为空，它将保持为空：

```javascript
  render() {
    return <CustomButton color={null} /> ; // props.color will remain null
  }
```

### `displayName`

该`displayName`字符串用于调试消息。通常，您不需要明确设置它，因为它是根据定义组件的函数或类的名称推断出来的。如果要为调试目的显示不同的名称或创建高阶组件时，可能需要显式设置它，请参阅包装显示名称以获取详细信息。

## 实例属性

### `props`

`this.props`包含由该组件的调用者定义的道具。有关道具的介绍，请参阅组件和道具。

特别是，它`this.props.children`是一个特殊的道具，通常由JSX表达式中的子标签定义，而不是标签本身。

### `state`

状态包含特定于此组件的数据，这些数据可能随时间而改变。状态是用户定义的，它应该是一个普通的 JavaScript 对象。

如果你不使用它`render()`，它不应该处于状态。例如，您可以将计时器ID直接放在实例上。

有关状态的更多信息，请参阅状态和生命周期。

不要`this.state`直接变异，因为`setState()`之后调用可能会取代你所做的变异。把`this.state`它看作是不可改变的。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com