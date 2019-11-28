# Handling Events

- 贡献者1人

  

使用React元素处理事件与处理DOM元素上的事件非常相似。有一些语法差异：

- React事件使用camelCase命名，而不是小写。

- 使用JSX，您将传递函数作为事件处理函数，而不是字符串。

例如，HTML：

```javascript
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

在React中略有不同：

```javascript
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

另一个区别是，您无法返回`false`以防止React中的默认行为。您必须`preventDefault`明确调用。例如，对于纯HTML，为了防止打开新页面的默认链接行为，您可以编写：

```javascript
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

在React中，这可能是：

```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

这里`e`是一个综合事件。React根据[W3C规范](https://www.w3.org/TR/DOM-Level-3-Events/)定义了这些合成事件，因此您不必担心跨浏览器兼容性。请参阅`SyntheticEvent`参考指南了解更多信息。

使用React时，通常不需要调用`addEventListener`添加侦听器到DOM元素后创建。相反，只需在元素初始呈现时提供侦听器。

当你使用[ES6类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)定义一个组件时，一个常见的模式是让一个事件处理器成为该类的一个方法。例如，这个`Toggle`组件呈现一个按钮，让用户在“ON”和“OFF”状态之间切换：

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

您必须小心`this`JSX回调的含义。在JavaScript中，类方法默认没有[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)。如果你忘记绑定`this.handleClick`并传递给它`onClick`，`this`将会在`undefined`实际调用该函数时出现。

这不是特定于反应的行为; 它是[JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)中[如何使用函数](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)的一部分。一般来说，如果你引用一个没有`()`后面的方法，比如`onClick={this.handleClick}`你应该绑定那个方法。

如果调用`bind`让你恼火，有两种方法可以解决这个问题。如果您使用的是实验性[公共类字段语法](https://babeljs.io/docs/plugins/transform-class-properties/)，则可以使用类字段来正确地绑定回调：

```javascript
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

这个语法在[Create React App中](https://github.com/facebookincubator/create-react-app)是默认启用的。

如果您不使用类字段语法，则可以在回调中使用[箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：

```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

这个语法的问题是每次`LoggingButton`渲染都会创建一个不同的回调函数。在大多数情况下，这很好。但是，如果将此回调作为道具传递给较低组件，则这些组件可能会进行额外的重新渲染。我们通常建议在构造函数中绑定或使用类字段语法来避免此类性能问题。

## 将参数传递给事件处理程序

在循环内部，通常需要将一个额外的参数传递给事件处理程序。例如，如果`id`是行ID，则以下任一项都可以工作：

```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

以上两行相同，分别使用[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)和[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)[`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind)。

在这两种情况下，`e`表示React事件的参数都将作为ID之后的第二个参数传递。使用箭头函数，我们必须明确地传递它，但是`bind`任何更多的参数都会自动转发。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com