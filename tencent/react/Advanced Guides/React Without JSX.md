# React Without JSX

JSX不是使用React的必要条件。如果您不想在编译环境中设置编译，在不使用JSX的情况下使用React特别方便。

每个JSX元素都只是调用的语法糖`React.createElement(component, props, ...children)`。所以，任何你能用JSX做的事情都可以用普通的JavaScript来完成。

例如，用JSX编写的这段代码：

```javascript
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.toWhat}</div>;
  }
}

ReactDOM.render(
  <Hello toWhat="World" />,
  document.getElementById('root')
);
```

可以编译为不使用JSX的代码：

```javascript
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);
```

如果您想了解更多JSX如何转换为JavaScript的示例，则可以尝试[使用联机的Babel编译器](https://babeljs.io/repl/#?presets=react&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyA)。

组件既可以作为字符串提供，也可以作为子类提供，也可以作为`React.Component`无状态组件的普通函数提供。

如果你厌倦了打字`React.createElement`太多，一种常见的模式是分配一个简写：

```javascript
const e = React.createElement;

ReactDOM.render(
  e('div', null, 'Hello World'),
  document.getElementById('root')
);
```

如果你使用这种简写形式`React.createElement`，在不使用JSX的情况下使用React几乎可以方便。

或者，你可以参考社区项目，如[`react-hyperscript`](https://github.com/mlmorg/react-hyperscript)和[`hyperscript-helpers`](https://github.com/ohanhi/hyperscript-helpers)其提供了一个更简洁的语法。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18