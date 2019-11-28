# Portals

门户网站提供了一种一流的方法来将孩子呈现到父组件的 DOM 层次结构之外的 DOM 节点中。

```javascript
ReactDOM.createPortal(child, container)
```

第一个参数（`child`）是任何可渲染的 React 子元素，例如元素，字符串或片段。第二个参数（`container`）是一个 DOM 元素。

## 用法

通常，当您从组件的 render 方法中返回一个元素时，它会作为最近的父节点的子元素装载到 DOM中：

```javascript
render() {
  // React mounts a new div and renders the children into it
  return (
    <div>
      {this.props.children}
    </div>
  );
}
```

但是，有时将子项插入 DOM 中的其他位置很有用：

```javascript
render() {
  // React does *not* create a new div. It renders the children into `domNode`.
  // `domNode` is any valid DOM node, regardless of its location in the DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode,
  );
}
```

门户的一个典型用例是当父组件具有`overflow: hidden`或`z-index`样式时，但您需要孩子直观地“分离”其容器。例如，对话框，悬浮卡片和工具提示。

> 注意：重要的是要记住，在使用门户时，您需要确保遵循适当的可访问性准则。

**在 CodePen 上试用它。**

## 事件通过门户冒泡

即使门户网站可以位于 DOM 树中的任何位置，但其行为与其他方式中的普通 React 子网站相似。像上下文这样的功能完全一样，无论孩子是否是门户网站，因为无论 *DOM 树中*的位置如何，门户网站仍然存在于 *React 树*中。

这包括事件冒泡。从门户内部触发的事件将传播到包含 *React 树*中的祖先，即使这些元素不是 *DOM 树中的*祖先。假设以下 HTML 结构：

```javascript
<html>
  <body>
    <div id="app-root"></div>
    <div id="modal-root"></div>
  </body>
</html>
```

一个`Parent`组件`#app-root`将能够从兄弟节点捕获未捕获的冒泡事件`#modal-root`。

```javascript
// These two containers are siblings in the DOM
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el,
    );
  }
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {clicks: 0};
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // This will fire when the button in Child is clicked,
    // updating Parent's state, even though button
    // is not direct descendant in the DOM.
    this.setState(prevState => ({
      clicks: prevState.clicks + 1
    }));
  }

  render() {
    return (
      <div onClick={this.handleClick}>
        <p>Number of clicks: {this.state.clicks}</p>
        <p>
          Open up the browser DevTools
          to observe that the button
          is not a child of the div
          with the onClick handler.
        </p>
        <Modal>
          <Child />
        </Modal>
      </div>
    );
  }
}

function Child() {
  // The click event on this button will bubble up to parent,
  // because there is no 'onClick' attribute defined
  return (
    <div className="modal">
      <button>Click</button>
    </div>
  );
}

ReactDOM.render(<Parent />, appRoot);
```

**在 CodePen 上试用它。**

捕获从父组件中的门户冒泡的事件允许开发更加灵活的抽象，这些抽象不是固有地依赖于门户。例如，如果您渲染`<Modal />`组件，则父级可以捕获其事件，而不管它是否使用门户实施。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18