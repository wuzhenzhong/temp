# Context

> 注意： `React.PropTypes`自从React v15.5以来，它已经进入了一个不同的包。请使用[该`prop-types`库，而不是](https://www.npmjs.com/package/prop-types)定义`contextTypes`。我们提供[了一个codemod脚本](https://reactjs.org/blog/2017/04/07/react-v15.5.0.html#migrating-from-react.proptypes)来自动化转换。

通过React，可以轻松跟踪通过React组件的数据流。当你看一个组件时，你可以看到哪些道具被传递，这使得你的应用程序很容易推理。

在某些情况下，您希望通过组件树传递数据，而不必在每个级别手动传递道具。您可以直接在React中使用强大的“上下文”API执行此操作。

## 为什么不使用上下文

绝大多数应用程序不需要使用上下文。

如果你希望你的应用程序稳定，不要使用上下文。这是一个实验性的API，它很可能在未来的React版本中被打破。

如果您不熟悉像[Redux](https://github.com/reactjs/redux)或[MobX](https://github.com/mobxjs/mobx)这样的状态管理库，请不要使用上下文。对于许多实际应用来说，这些库和它们的React绑定对于管理与许多组件相关的状态是一个很好的选择。Redux很可能是解决问题的正确解决方案，而不是正确的解决方案。

如果您不是经验丰富的React开发人员，请不要使用上下文。通常只有使用道具和状态才能实现功能。

如果您坚持使用上下文而不管这些警告，请尝试将上下文的使用隔离到一个小区域，并尽可能避免直接使用上下文API，以便在API更改时更容易升级。

## 如何使用上下文

假设你有一个像这样的结构：

```javascript
class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.props.color}}>
        {this.props.children}
      </button>
    );
  }
}

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button color={this.props.color}>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  render() {
    const color = "purple";
    const children = this.props.messages.map((message) =>
      <Message text={message.text} color={color} />
    );
    return <div>{children}</div>;
  }
}
```

在这个例子中，我们手动穿过一个`color`道具来适当地设计`Button`和`Message`组件。使用上下文，我们可以自动将它传递给树：

```javascript
import PropTypes from 'prop-types';

class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.context.color}}>
        {this.props.children}
      </button>
    );
  }
}

Button.contextTypes = {
  color: PropTypes.string
};

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  getChildContext() {
    return {color: "purple"};
  }

  render() {
    const children = this.props.messages.map((message) =>
      <Message text={message.text} />
    );
    return <div>{children}</div>;
  }
}

MessageList.childContextTypes = {
  color: PropTypes.string
};
```

通过添加`childContextTypes`和`getChildContext`到`MessageList`（上下文提供），阵营传递的子树中向下自动信息和任何组分（在这种情况下，`Button`）可以通过定义访问它`contextTypes`。

如果`contextTypes`没有定义，那么`context`将是一个空对象。

## 亲子合作

Context还可以让你建立一个父母和孩子交流的API。例如，以这种方式工作的一个库是[React Router V4](https://reacttraining.com/react-router)：

```javascript
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr />

      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/topics" component={Topics} />
    </div>
  </Router>
);
```

通过从经过了一些信息`Router`部分，每个`Link`和`Route`可回传送到含有`Router`。

在使用与此类似的API构建组件之前，请考虑是否有更清晰的替代方案。例如，如果您愿意，您可以将整个React组件作为道具传递。

## 在生命周期方法中引用上下文

如果`contextTypes`在组件内定义，则以下生命周期方法将收到附加参数，即`context`对象：

- `constructor(props, context)`

- `componentWillReceiveProps(nextProps, nextContext)`

- `shouldComponentUpdate(nextProps, nextState, nextContext)`

- `componentWillUpdate(nextProps, nextState, nextContext)`

> 注意：截至React 16，`componentDidUpdate`不再收到`prevContext`。

## 在无状态功能组件中引用上下文

无状态的功能组件也可以引用，`context`如果`contextTypes`被定义为该功能的属性。以下代码显示了一个`Button`写入无状态功能组件的组件。

```javascript
import PropTypes from 'prop-types';

const Button = ({children}, context) =>
  <button style={{background: context.color}}>
    {children}
  </button>;

Button.contextTypes = {color: PropTypes.string};
```

## 更新上下文

不要这样做。

React有一个API来更新上下文，但它基本上被打破了，你不应该使用它。

`getChildContext`功能将在状态或道具改变时被调用。为了更新上下文中的数据，使用`this.setState`。触发本地状态更新。这将触发一个新的环境，并且变化将被孩子接收。

```javascript
import PropTypes from 'prop-types';

class MediaQuery extends React.Component {
  constructor(props) {
    super(props);
    this.state = {type:'desktop'};
  }

  getChildContext() {
    return {type: this.state.type};
  }

  componentDidMount() {
    const checkMediaQuery = () => {
      const type = window.matchMedia("(min-width: 1025px)").matches ? 'desktop' : 'mobile';
      if (type !== this.state.type) {
        this.setState({type});
      }
    };

    window.addEventListener('resize', checkMediaQuery);
    checkMediaQuery();
  }

  render() {
    return this.props.children;
  }
}

MediaQuery.childContextTypes = {
  type: PropTypes.string
};
```

的问题是，如果由组分变化，即使用该值后代提供上下文值将不会如果中间父返回更新`false`从`shouldComponentUpdate`。这完全无法控制使用上下文的组件，所以基本上无法可靠地更新上下文。[这篇博文](https://medium.com/@mweststrate/how-to-safely-use-react-context-b7e343eff076)很好地解释了为什么这是一个问题，以及如何解决这个问题。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18