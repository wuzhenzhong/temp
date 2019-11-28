# Test Renderer

**输入**

```javascript
import TestRenderer from 'react-test-renderer'; // ES6
const TestRenderer = require('react-test-renderer'); // ES5 with npm
```

## 概观

该包提供了一个React渲染器，可用于将React组件呈现为纯JavaScript对象，而不依赖于DOM或本地移动环境。

从本质上讲，这个软件包可以轻松获取由React DOM或React Native组件呈现的平台视图层次结构（类似于DOM树）的快照，而无需使用浏览器或[jsdom](https://github.com/tmpvar/jsdom)。

例：

```javascript
import TestRenderer from 'react-test-renderer';

function Link(props) {
  return <a href={props.page}>{props.children}</a>;
}

const testRenderer = TestRenderer.create(
  <Link page="https://www.facebook.com/">Facebook</Link>
);

console.log(testRenderer.toJSON());
// { type: 'a',
//   props: { href: 'https://www.facebook.com/' },
//   children: [ 'Facebook' ] }
```

您可以使用Jest的快照测试功能自动将JSON树的副本保存到文件中，并检查您的测试是否未更改：[了解更多信息](http://facebook.github.io/jest/blog/2016/07/27/jest-14.html)。

您还可以遍历输出以查找特定节点并对其进行断言。

```javascript
import TestRenderer from 'react-test-renderer';

function MyComponent() {
  return (
    <div>
      <SubComponent foo="bar" />
      <p className="my">Hello</p>
    </div>
  )
}

function SubComponent() {
  return (
    <p className="sub">Sub</p>
  );
}

const testRenderer = TestRenderer.create(<MyComponent />);
const testInstance = testRenderer.root;

expect(testInstance.findByType(SubComponent).props.foo).toBe('bar');
expect(testInstance.findByProps({className: "sub"}).children).toEqual(['Sub']);
```

### TestRenderer

- `TestRenderer.create()`TestRenderer实例

- `testRenderer.toJSON()`

- `testRenderer.toTree()`

- `testRenderer.update()`

- `testRenderer.unmount()`

- `testRenderer.getInstance()`

- `testRenderer.root`

### TestInstance

- `testInstance.find()`

- `testInstance.findByType()`

- `testInstance.findByProps()`

- `testInstance.findAll()`

- `testInstance.findAllByType()`

- `testInstance.findAllByProps()`

- `testInstance.instance`

- `testInstance.type`

- `testInstance.props`

- `testInstance.parent`

- `testInstance.children`

## 参考

### `TestRenderer.create()`

```javascript
TestRenderer.create(element, options);
```

`TestRenderer`使用传递的React元素创建一个实例。它不使用真正的DOM，但它仍然将组件树完全渲染到内存中，因此您可以对其进行断言。返回的实例具有以下方法和属性。

### `testRenderer.toJSON()`

```javascript
testRenderer.toJSON()
```

返回表示渲染树的对象。此树只包含特定于平台的节点，例如`<div>`or `<View>`和它们的道具，但不包含任何用户编写的组件。这对于[快照测试](http://facebook.github.io/jest/docs/en/snapshot-testing.html#snapshot-testing-with-jest)非常方便。

### `testRenderer.toTree()`

```javascript
testRenderer.toTree()
```

返回表示渲染树的对象。不同的是`toJSON()`，该表示比由其提供的表示更详细`toJSON()`，并且包括用户编写的组件。除非要在测试渲染器上编写自己的断言库，否则可能不需要此方法。

### `testRenderer.update()`

```javascript
testRenderer.update(element)
```

用新的根元素重新渲染内存树。这模拟了根上的React更新。如果新元素与前一个元素具有相同的类型和关键字，则树会被更新; 否则，它将重新安装一棵新树。

### `testRenderer.unmount()`

```javascript
testRenderer.unmount()
```

卸载内存树，触发适当的生命周期事件。

### `testRenderer.getInstance()`

```javascript
testRenderer.getInstance()
```

如果可用，返回对应于根元素的实例。如果根元素是一个功能组件，因为它们没有实例，这将不起作用。

### `testRenderer.root`

```javascript
testRenderer.root
```

返回对测试树中特定节点断言很有用的根“测试实例”对象。您可以使用它来查找下面更深的其他“测试实例”。

### `testInstance.find()`

```javascript
testInstance.find(test)
```

找到一个单一的后代测试实例为其`test(testInstance)`返回`true`。如果`test(testInstance)`没有完全返回`true`一个测试实例，则会引发错误。

### `testInstance.findByType()`

```javascript
testInstance.findByType(type)
```

使用提供的查找单个后代测试实例`type`。如果提供的测试实例不完全相同`type`，则会引发错误。

### `testInstance.findByProps()`

```javascript
testInstance.findByProps(props)
```

使用提供的查找单个后代测试实例`props`。如果提供的测试实例不完全相同`props`，则会引发错误。

### `testInstance.findAll()`

```javascript
testInstance.findAll(test)
```

找到所有的`test(testInstance)`回报测试实例`true`。

### `testInstance.findAllByType()`

```javascript
testInstance.findAllByType(type)
```

用提供的查找所有后代测试实例`type`。

### `testInstance.findAllByProps()`

```javascript
testInstance.findAllByProps(props)
```

用提供的查找所有后代测试实例`props`。

### `testInstance.instance`

```javascript
testInstance.instance
```

组件实例对应于此测试实例。它仅适用于类组件，因为功能组件没有实例。它匹配`this`给定组件内的值。

### `testInstance.type`

```javascript
testInstance.type
```

与此测试实例相对应的组件类型。例如，一个`<Button />`组件有一个类型`Button`。

### `testInstance.props`

```javascript
testInstance.props
```

这个测试实例对应的道具。例如，一个`<Button size="small />`组件具有`{size: 'small'}`道具。

### `testInstance.parent`

```javascript
testInstance.parent
```

此测试实例的父测试实例。

### `testInstance.children`

```javascript
testInstance.children
```

子们测试这个测试实例的实例。

## 思路

您可以传递`createNodeMock`函数`TestRenderer.create`作为选项，它允许自定义模拟参考。`createNodeMock`接受当前元素并返回一个模拟参考对象。当您测试依赖于参考的组件时，这很有用。

```javascript
import TestRenderer from 'react-test-renderer';

class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.input = null;
  }
  componentDidMount() {
    this.input.focus();
  }
  render() {
    return <input type="text" ref={el => this.input = el} />
  }
}

let focused = false;
TestRenderer.create(
  <MyComponent />,
  {
    createNodeMock: (element) => {
      if (element.type === 'input') {
        // mock a focus function
        return {
          focus: () => {
            focused = true;
          }
        };
      }
      return null;
    }
  }
);
expect(focused).toBe(true);
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com