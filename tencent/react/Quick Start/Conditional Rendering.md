# Conditional Rendering

在React中，您可以创建封装您所需行为的独特组件。然后，您可以仅渲染其中的一部分，具体取决于应用程序的状态。

React中的条件呈现与条件在JavaScript中的工作方式相同。使用JavaScript运算符[`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)或[条件运算符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)来创建表示当前状态的元素，并让React更新UI以匹配它们。

考虑这两个组件：

```javascript
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

我们将创建一个`Greeting`组件，根据用户是否登录显示这些组件中的任何一个：

```javascript
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

此示例根据`isLoggedIn`prop 的值呈现不同的问候语。

### 元素变量

您可以使用变量来存储元素。这可以帮助您有条件地渲染组件的一部分，而其余输出不会更改。

考虑这两个代表注销和登录按钮的新组件：

```javascript
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

在下面的例子中，我们将创建一个名为有状态的组件`LoginControl`。

它将呈现`<LoginButton />`或`<LogoutButton />`根据其当前状态。它也会呈现一个`<Greeting />`来自前面的例子：

```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

虽然声明变量并使用`if`语句是条件渲染组件的好方法，但有时您可能想使用更短的语法。有几种方法可以在JSX中内联条件，如下所述。

### 内联如果使用逻辑&&运算符

您可以通过将它们包裹在花括号中来嵌入JSX中的任何表达式。这包括JavaScript逻辑`&&`运算符。它有条件地包含一个元素：

```javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

它的工作原理是因为在JavaScript中，`true && expression`总是评估`expression`并`false && expression`总是评估`false`。

因此，如果条件为`true`，则紧接着的元素`&&`将出现在输出中。如果是的话`false`，React会忽略并跳过它。

### 与条件运算符一起内联If- Else

另一种有条件地渲染元素的方法是使用JavaScript条件运算符[`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。

在下面的例子中，我们用它来有条件地渲染一小块文本。

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

它也可以用于较大的表达式，虽然它不太明显是怎么回事：

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

就像在JavaScript中一样，您可以根据您和您的团队认为更具可读性选择适当的样式。还要记住，只要条件变得太复杂，它可能是提取组件的好时机。

### 防止组件呈现

在极少数情况下，您可能希望组件能够隐藏自身，即使它是由另一个组件渲染的。要做到这一点，`null`而不是它的渲染输出。

在下面的例子中，`<WarningBanner />`根据被调用的道具的值来渲染`warn`。如果道具的值是`false`，那么组件不渲染：

```javascript
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

`null`从组件的`render`方法返回不会影响组件生命周期方法的触发。例如，`componentWillUpdate`并且`componentDidUpdate`仍然会被调用。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18