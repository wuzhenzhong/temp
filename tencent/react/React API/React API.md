# React API

`React`是React库的入口点。如果您从`<script>`标记加载React ，则这些顶级API可在`React`全局中使用。如果你使用npm的ES6，你可以写`import React from 'react'`。如果你使用npm的ES5，你可以写`var React = require('react')`。

## 概观

### 组件

React组件让你可以将UI分成独立的，可重用的部分，并且可以独立思考每个部分。React组件可以通过子类化`React.Component`或`React.PureComponent`。

- `React.Component`
- `React.PureComponent`

如果您不使用ES6类，则可以使用该`create-react-class`模块。有关更多信息，请参阅使用不带ES6的React。

### 创建React元素

我们建议使用JSX来描述你的UI应该是什么样子。每个JSX元素都只是调用的语法糖`React.createElement()`。如果您使用JSX，通常不会直接调用以下方法。

- `createElement()`
- `createFactory()`

有关更多信息，请参阅使用不带JSX的React。

### 转换元素

`React` 提供了几个用于操作元素的API：

- `cloneElement()`
- `isValidElement()`
- `React.Children`

### 片段

`React` 还提供了一个组件，可以在不使用包装的情况下呈现多个元素。

- `React.Fragment`

## 参考

### `React.Component`

`React.Component`是React组件在使用[ES6类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)定义时的基[类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

请参阅React.Component API参考以获取与基`React.Component`类相关的方法和属性列表。

### `React.PureComponent`

`React.PureComponent`类似于`React.Component`。它们之间的区别在于`React.Component`它没有实现`shouldComponentUpdate()`，但是`React.PureComponent`实现了它与浅色道具和状态比较。

如果您的React组件的`render()`函数在给定相同的道具和状态的`React.PureComponent`情况下呈现相同的结果，则可以在某些情况下用于提高性能。

> 需要注意 `React.PureComponent`的`shouldComponentUpdate()`只有浅浅的对象进行比较。如果这些包含复杂的数据结构，则可能会对更深的差异产生假阴性结果。只有`PureComponent`在您期望拥有简单的道具和状态时才会扩展，或者`forceUpdate()`在知道深层数据结构发生变化时使用。或者，考虑使用[不可变对象](https://facebook.github.io/immutable-js/)以便快速比较嵌套数据。此外，`React.PureComponent`的`shouldComponentUpdate()`跳过整个组件树道具更新。确保所有儿童组件都是“纯”的。

### `createElement()`

```javascript
React.createElement(
  type,
  [props],
  [...children]
)
```

创建并返回给定类型的新React元素。type参数可以是标记名称字符串（如`'div'`或`'span'`），React组件类型（类或函数）或React片段类型。

用JSX编写的代码将被转换为使用`React.createElement()`。`React.createElement()`如果您使用JSX，通常不会直接调用。请参阅无JSX的React以了解更多信息。

### `cloneElement()`

```javascript
React.cloneElement(
  element,
  [props],
  [...children]
)
```

以克隆和返回一个新的React元素`element`为起点。由此产生的元素将具有原始元素的道具，新道具融合得很浅。新的孩子将取代现有的孩子。`key`并`ref`从原始元素中保存下来。

`React.cloneElement()` 几乎相当于：

```javascript
<element.type {...element.props} {...props}>{children}</element.type>
```

但是，它也保留了`ref`s。这意味着如果你得到一个带着`ref`它的孩子，你不会意外地从你的祖先那里偷走它。您将获得与`ref`新元素相同的附加信息。

此API是作为已弃用的替代品而引入的`React.addons.cloneWithProps()`。

### `createFactory()`

```javascript
React.createFactory(type)
```

返回一个生成给定类型的React元素的函数。类似的`React.createElement()`，type参数可以是标签名字符串（如`'div'`or `'span'`），React组件类型（类或函数）或React片段类型。

这个帮助程序被认为是遗留的，我们鼓励您使用JSX或`React.createElement()`直接使用。

`React.createFactory()`如果您使用JSX，通常不会直接调用。请参阅无JSX的React以了解更多信息。

### `isValidElement()`

```javascript
React.isValidElement(object)
```

验证对象是一个React元素。返回`true`或`false`。

### `React.Children`

`React.Children`提供用于处理`this.props.children`不透明数据结构的实用程序。

#### `React.Children.map`

```javascript
React.Children.map(children, function[(thisArg)])
```

上调用包含在每一个直系子的功能`children`与`this`设置为`thisArg`。如果`children`是键控片段或数组，它将被遍历：该函数永远不会传递容器对象。如果孩子`null`或`undefined`返回`null`或`undefined`而不是一个数组。

#### `React.Children.forEach`

```javascript
React.Children.forEach(children, function[(thisArg)])
```

喜欢`React.Children.map()`但不返回数组。

#### `React.Children.count`

```javascript
React.Children.count(children)
```

返回组件的总数`children`，等于回调传递给`map`或`forEach`将被调用的次数。

#### `React.Children.only`

```javascript
React.Children.only(children)
```

验证`children`只有一个孩子（一个React元素）并返回它。否则，此方法会引发错误。

> 注意： `React.Children.only()`不接受返回值，`React.Children.map()`因为它是一个数组而不是React元素。

#### `React.Children.toArray`

```javascript
React.Children.toArray(children)
```

`children`以分配给每个子项的键的平面数组形式返回不透明的数据结构。如果你想在你的渲染方法中操作子集合，这很有用，特别是如果你想在重新排序或切片`this.props.children`之前将其传递下来。

> 注意： `React.Children.toArray()`在展平子列表时，更改键以保留嵌套数组的语义。也就是说，`toArray`为返回数组中的每个键添加前缀，以便将每个元素的键范围限定为包含它的输入数组。

### `React.Fragment`

该`React.Fragment`组件允许您在`render()`方法中返回多个元素而不创建额外的DOM元素：

```javascript
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```

您也可以使用简写`<></>`语法。有关更多信息，请参阅[React v16.2.0：改进对片段的支持](https://reactjs.org/blog/2017/11/28/react-v16.2.0-fragment-support.html)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com