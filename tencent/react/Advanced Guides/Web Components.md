# Web Components

React和[Web组件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)的构建是为了解决不同的问题。Web组件为可重用组件提供了强大的封装，而React提供了一个声明库来保持DOM与数据同步。这两个目标是相辅相成的。作为开发人员，您可以在Web组件中自由使用React，或者在React中使用Web组件，或者同时使用这两种组件。

大多数使用React的人不使用Web组件，但可能需要使用Web组件，尤其是在使用使用Web组件编写的第三方UI组件时。

## 在React中使用Web组件

[纠错](javascript:;)

```javascript
class HelloMessage extends React.Component {
  render() {
    return <div>Hello <x-search>{this.props.name}</x-search>!</div>;
  }
}
```

> 注意：Web组件经常公开一个命令式API。例如，一个`video`Web组件可能会公开`play()`并`pause()`起作用。要访问Web组件的命令式API，您需要使用ref直接与DOM节点交互。如果您使用的是第三方Web组件，最好的解决方案是编写一个React组件，该组件可用作Web组件的包装。Web组件发出的事件可能无法通过React渲染树正确传播。您将需要手动附加事件处理程序来处理React组件中的这些事件。

一个普遍的混淆是Web组件使用“class”而不是“className”。

```javascript
function BrickFlipbox() {
  return (
    <brick-flipbox class="demo">
      <div>front</div>
      <div>back</div>
    </brick-flipbox>
  );
}
```

## 在您的Web组件中使用React

```javascript
class XSearch extends HTMLElement {
  connectedCallback() {
    const mountPoint = document.createElement('span');
    this.attachShadow({ mode: 'open' }).appendChild(mountPoint);

    const name = this.getAttribute('name');
    const url = 'https://www.google.com/search?q=' + encodeURIComponent(name);
    ReactDOM.render(<a href={url}>{name}</a>, mountPoint);
  }
}
customElements.define('x-search', XSearch);
```

> 注意：如果您使用Babel转换类，此代码**将**不起作用。请参阅[此问题](https://github.com/w3c/webcomponents/issues/587)以进行讨论。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18