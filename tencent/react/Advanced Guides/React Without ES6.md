# React Without ES6

通常你会将一个React组件定义为一个普通的JavaScript类：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

如果您还没有使用ES6，则可以使用该`create-react-class`模块：

[纠错](javascript:;)

```javascript
var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
```

ES6类的API类似于`createReactClass()`少数例外。

## 声明默认道具

使用函数和ES6类`defaultProps`定义为组件本身的属性：

```javascript
class Greeting extends React.Component {
  // ...
}

Greeting.defaultProps = {
  name: 'Mary'
};
```

与`createReactClass()`，你需要定义`getDefaultProps()`为传递对象的函数：

```javascript
var Greeting = createReactClass({
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },

  // ...

});
```

## 设置初始状态

在ES6类中，你可以通过`this.state`在构造函数中赋值来定义初始状态：

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
  // ...
}
```

与`createReactClass()`，你必须提供一个单独的`getInitialState`方法，返回初始状态：

```javascript
var Counter = createReactClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // ...
});
```

## 自动绑定

在声明为ES6类的React组件中，方法遵循与常规ES6类相同的语义。这意味着它们不会自动绑定`this`到实例。您必须`.bind(this)`在构造函数中明确使用：

```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
    // This line is important!
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    alert(this.state.message);
  }

  render() {
    // Because `this.handleClick` is bound, we can use it as an event handler.
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

有了`createReactClass()`这个，没有必要，因为它绑定了所有的方法：

```javascript
var SayHello = createReactClass({
  getInitialState: function() {
    return {message: 'Hello!'};
  },

  handleClick: function() {
    alert(this.state.message);
  },

  render: function() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
});
```

这意味着编写ES6类为事件处理程序提供了更多样板代码，但是在大型应用程序中，性能稍好一些。

如果样板代码对您来说太不吸引人，您可以使用Babel 启用**实验性的“** [类属性”](https://babeljs.io/docs/plugins/transform-class-properties/)语法提议：

```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
  }
  // WARNING: this syntax is experimental!
  // Using an arrow here binds the method:
  handleClick = () => {
    alert(this.state.message);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

请注意，上面的语法是**实验性的**，语法可能会更改，或者提案可能不会将其添加到语言中。

如果你想安全地玩，你有几个选择：

- 在构造函数中绑定方法。

- 使用箭头功能，例如`onClick={(e) => this.handleClick(e)}`。

- 继续使用`createReactClass`。

## 混入

> **注意：** ES6没有任何mixin支持。因此，当您使用ES6类的React时，不支持mixin。 **我们在使用mixins的代码库中也发现了很多问题，** [**并且不建议在新代码中使用它们**](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html)**。** 本部分仅供参考。

有时候，非常不同的组件可能会共享一些通用功能 这些有时被称为[交叉担忧](https://en.wikipedia.org/wiki/Cross-cutting_concern)。`createReactClass`可以让您使用旧`mixins`系统。

一个常见的用例是想要在一段时间间隔内自行更新的组件。它很容易使用`setInterval()`，但是当你不再需要它来节省内存时，取消间隔很重要。React提供了生命周期方法，可以让您知道组件即将创建或销毁的时间。让我们创建一个简单的混合使用这些方法来提供一个简单的`setInterval()`函数，当你的组件被销毁时它会自动清理。

```javascript
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.forEach(clearInterval);
  }
};

var createReactClass = require('create-react-class');

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // Use the mixin
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // Call a method on the mixin
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

ReactDOM.render(
  <TickTock />,
  document.getElementById('example')
);
```

如果一个组件使用多个mixin，并且多个mixin定义了相同的生命周期方法（即当组件被销毁时，多个mixin想要做一些清理），所有的生命周期方法都会被保证调用。列出在mixin中以mixin顺序运行的方法，然后列出组件上的方法调用。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18