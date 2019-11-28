# Test Utilities

**输入**

```javascript
import ReactTestUtils from 'react-dom/test-utils'; // ES6
var ReactTestUtils = require('react-dom/test-utils'); // ES5 with npm
```

## 概观

`ReactTestUtils`使您可以轻松地在您选择的测试框架中测试React组件。在Facebook我们使用[Jest](https://facebook.github.io/jest/)进行无痛JavaScript测试。通过Jest网站的[React Tutorial](http://facebook.github.io/jest/docs/en/tutorial-react.html#content)了解如何开始使用Jest 。

> 注意：Airbnb已经发布了一个名为Enzyme的测试工具，它可以很容易地断言，操纵和遍历React Components的输出。如果您决定使用单元测试实用程序与Jest或任何其他测试运行器一起使用，则值得检查：[http](http://airbnb.io/enzyme/) : [//airbnb.io/enzyme/](http://airbnb.io/enzyme/)

- `Simulate`

- `renderIntoDocument()`

- `mockComponent()`

- `isElement()`

- `isElementOfType()`

- `isDOMComponent()`

- `isCompositeComponent()`

- `isCompositeComponentWithType()`

- `findAllInRenderedTree()`

- `scryRenderedDOMComponentsWithClass()`

- `findRenderedDOMComponentWithClass()`

- `scryRenderedDOMComponentsWithTag()`

- `findRenderedDOMComponentWithTag()`

- `scryRenderedComponentsWithType()`

- `findRenderedComponentWithType()`

## 参考

## 浅的渲染

在为React编写单元测试时，浅层渲染可能会有所帮助。浅层渲染使您可以渲染“一层深度”的组件，并确定其渲染方法返回的事实，而不必担心未实例化或渲染的子组件的行为。这不需要DOM。

> 注意：浅呈现器已移至`react-test-renderer/shallow`。在参考页面上了解更多关于浅层渲染的信息。

## 其他实用程序

### `Simulate`

```javascript
Simulate.{eventName}(
  element,
  [eventData]
)
```

使用可选`eventData`事件数据模拟DOM节点上的事件分派。

`Simulate` 对于React理解的每个事件都有一个方法。

**点击一个元素**

```javascript
// <button ref={(node) => this.button = node}>...</button>
const node = this.button;
ReactTestUtils.Simulate.click(node);
```

**更改输入字段的值，然后按ENTER键。**

```javascript
// <input ref={(node) => this.textInput = node} />
const node = this.textInput;
node.value = 'giraffe';
ReactTestUtils.Simulate.change(node);
ReactTestUtils.Simulate.keyDown(node, {key: "Enter", keyCode: 13, which: 13});
```

> 注意您必须提供您在组件中使用的任何事件属性（例如，keyCode，等等），因为React不会为您创建任何这些属性。

### `renderIntoDocument()`

```javascript
renderIntoDocument(element)
```

将React元素渲染到文档中的分离DOM节点中。**这个功能需要一个DOM。**

> 注意：**在**导入**之前**`window`，您将需要拥有`window.document`并`window.document.createElement`全局可用。否则React会认为它不能访问DOM，而像这样的方法将无法工作。 `ReactsetState`

### `mockComponent()`

```javascript
mockComponent(
  componentClass,
  [mockTagName]
)
```

将模拟的组件模块传递给此方法，以便使用有用的方法来扩充它，以便将其用作虚拟React组件。不像往常那样渲染，该组件将变成包含任何提供的子项的简单`<div>`（或提供其他标签`mockTagName`）。

> 注意： `mockComponent()`是一个传统的API。我们建议使用浅色渲染或[`jest.mock()`](https://facebook.github.io/jest/docs/en/tutorial-react-native.html#mock-native-modules-using-jestmock)替代。

### `isElement()`

```javascript
isElement(element)
```

返回`true`是否`element`有任何React元素。

### `isElementOfType()`

```javascript
isElementOfType(
  element,
  componentClass
)
```

返回`true`if `element`是React元素的类型是React `componentClass`。

### `isDOMComponent()`

```javascript
isDOMComponent(instance)
```

返回`true`是否`instance`是DOM组件（如a `<div>`或`<span>`）。

### `isCompositeComponent()`

```javascript
isCompositeComponent(instance)
```

返回`true`是否`instance`是用户定义的组件，例如类或函数。

### `isCompositeComponentWithType()`

```javascript
isCompositeComponentWithType(
  instance,
  componentClass
)
```

`true`如果`instance`是类型为React的组件，则返回`componentClass`。

### `findAllInRenderedTree()`

```javascript
findAllInRenderedTree(
  tree,
  test
)
```

遍历所有组件`tree`和积累的所有组件，其中`test(component)`的`true`。这本身并不是很有用，但它被用作其他测试应用程序的原语。

### `scryRenderedDOMComponentsWithClass()`

```javascript
scryRenderedDOMComponentsWithClass(
  tree,
  className
)
```

在呈现的树中查找与类名匹配的DOM组件的所有DOM元素`className`。

### `findRenderedDOMComponentWithClass()`

```javascript
findRenderedDOMComponentWithClass(
  tree,
  className
)
```

喜欢`scryRenderedDOMComponentsWithClass()`但预计会有一个结果，并返回该结果，或者如果除了一个之外还有其他任何匹配项，则会抛出异常。

### `scryRenderedDOMComponentsWithTag()`

```javascript
scryRenderedDOMComponentsWithTag(
  tree,
  tagName
)
```

查找渲染树中组件的所有DOM元素，这些组件是标记名称匹配的DOM组件`tagName`。

### `findRenderedDOMComponentWithTag()`

```javascript
findRenderedDOMComponentWithTag(
  tree,
  tagName
)
```

喜欢`scryRenderedDOMComponentsWithTag()`但预计会有一个结果，并返回该结果，或者如果除了一个之外还有其他任何匹配项，则会抛出异常。

### `scryRenderedComponentsWithType()`

```javascript
scryRenderedComponentsWithType(
  tree,
  componentClass
)
```

查找类型等于的组件的所有实例`componentClass`。

### `findRenderedComponentWithType()`

```javascript
findRenderedComponentWithType(
  tree,
  componentClass
)
```

相同`scryRenderedComponentsWithType()`但期望得到一个结果并返回那一个结果，或者如果除了一个之外还有其他任何匹配，则抛出异常。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com