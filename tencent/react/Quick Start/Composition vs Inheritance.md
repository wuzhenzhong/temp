# Composition vs Inheritance

React具有强大的组合模型，我们推荐使用组合而不是继承来重用组件之间的代码。

在本节中，我们将考虑几个React经常需要继承的开发人员的问题，并展示如何用组合来解决它们。

## 遏制

有些组件不提前知道他们的孩子。这对于像`Sidebar`或`Dialog`代表通用“盒子”的组件尤其常见。

我们建议这些组件使用特殊的`children`道具将子元素直接传递到它们的输出中：

```javascript
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

这可以让其他组件通过嵌套JSX将任意子对象传递给它们：

```javascript
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

[在CodePen上试用。](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)

`<FancyBorder>`JSX标签内的任何东西都会`FancyBorder`作为`children`道具传入组件。由于`FancyBorder`渲染`{props.children}`内部`<div>`，传递的元素出现在最终的输出中。

虽然这种情况不太常见，但有时您可能需要在组件中出现多个“漏洞”。在这种情况下，你可以拿出你自己的约定，而不是使用`children`：

```javascript
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

[在CodePen上试用。](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)

反应相似的元素`<Contacts />`和`<Chat />`只是对象，这样你就可以通过他们像任何其他数据的道具。

## 专业化

有时我们会将组件视为其他组件的“特例”。例如，我们可以说a `WelcomeDialog`是一个特例`Dialog`。

在React中，这也可以通过组合来实现，其中一个更“特定”的组件呈现更“通用”的组件，并使用道具进行配置：

```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

对于定义为类的组件，组合的作用同样好：

```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)

## 那么继承呢？

在Facebook上，我们在数千个组件中使用React，并且我们还没有发现任何建议创建组件继承层次结构的用例。

道具和构图为您提供了所有需要的灵活性，以明确和安全的方式自定义组件的外观和行为。请记住，组件可以接受任意道具，包括原始值，React元素或函数。

如果您想在组件之间重用非UI功能，我们建议将其解压缩到单独的JavaScript模块中。组件可以将其导入并使用该函数，对象或类，而不对其进行扩展。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18