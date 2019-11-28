# Refs and the DOM

在典型的React数据流中，道具是父组件与子项交互的唯一方式。要修改一个孩子，你需要用新的道具重新渲染它。但是，在少数情况下，您需要在典型数据流之外强制修改子级。要修改的孩子可以是React组件的一个实例，也可以是DOM元素。对于这两种情况，React都提供了逃生舱口。

### 何时使用参考

对于refs有几个很好的用例：

- 管理焦点，文本选择或媒体播放。

- 触发命令式动画。

- 与第三方DOM库集成。

避免将ref用于任何可以通过声明完成的事情。

例如，不要在组件上暴露`open()`和使用`close()`方法，而要将prop `Dialog`传递`isOpen`给它。

### 不要过度使用参考文献

你的第一个倾向可能是使用引用来在应用程序中“发生事情”。如果是这种情况，请花一点时间，更仔细地考虑组件层次结构中应该拥有哪些状态。通常情况下，很显然，“拥有”该州的适当位置在层次结构中处于较高水平。有关这方面的示例，请参阅提升状态指南。

### 向DOM元素添加参考

React支持可以附加到任何组件的特殊属性。该`ref`属性采用回调函数，并且在组件挂载或卸载后立即执行回调。

当在`ref`HTML元素上使用该属性时，该`ref`回调接收基础DOM元素作为其参数。例如，此代码使用`ref`回调来存储对DOM节点的引用：

```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    this.textInput.focus();
  }

  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in an instance field (for example, this.textInput).
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

`ref`当组件装入时，React将使用DOM元素调用回调，并`null`在卸载时调用它。`ref`回调之前被调用`componentDidMount`或`componentDidUpdate`生命周期钩。

使用`ref`回调来设置类的属性是访问DOM元素的常用模式。首选的方法是在`ref`回调中设置属性，如上例所示。甚至有更短的写法：`ref={input => this.textInput = input}`。

### 向类组件添加一个Ref

当在`ref`声明为类的自定义组件上使用该属性时，`ref`回调接收组件的已装入实例作为其参数。例如，如果我们想要包装`CustomTextInput`上述内容以模拟安装后立即点击它：

```javascript
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

请注意，这仅适用于`CustomTextInput`声明为类的情况：

```javascript
class CustomTextInput extends React.Component {
  // ...
}
```

### 参考和功能组件

**您不能** **在功能组件上使用该属性，**因为它们没有实例：**ref**

```javascript
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  render() {
    // This will *not* work!
    return (
      <MyFunctionalComponent
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

如果需要引用组件，则应该将组件转换为类，就像在需要生命周期方法或状态时一样。

但是，只要您引用DOM元素或类组件，就可以**在功能组件内使用该** **属性**：**ref**

```javascript
function CustomTextInput(props) {
  // textInput must be declared here so the ref callback can refer to it
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}
```

### 将DOM Refs公开给父组件

在极少数情况下，您可能想要从父组件访问子节点的DOM节点。通常不建议这样做，因为它打破了组件封装，但它偶尔可用于触发焦点或测量子DOM节点的大小或位置。

虽然您可以向子组件添加ref，但这不是理想的解决方案，因为您只会获取组件实例而不是DOM节点。此外，这不适用于功能组件。

相反，在这种情况下，我们建议在儿童身上暴露特殊的道具。这个孩子会使用一个任意名称（例如`inputRef`）的函数道具，并将其作为一个`ref`属性附加到DOM节点。这可以让父母通过中间组件将其ref回调传递给子节点的DOM节点。

这适用于类和功能组件。

```javascript
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

在上面的例子中，`Parent`将它的ref回调作为一个`inputRef`道具传递给`CustomTextInput`，并且`CustomTextInput`将相同的函数作为特殊`ref`属性传递给`<input>`。其结果是，`this.inputElement`在`Parent`将被设置为对应于所述DOM节点`<input>`中的元素`CustomTextInput`。

请注意，`inputRef`上例中prop 的名称没有特殊含义，因为它是常规组件prop。然而，使用`ref`属性`<input>`本身很重要，因为它告诉React将ref附加到它的DOM节点。

即使它`CustomTextInput`是一个功能组件，它也可以工作。与`ref`只能为DOM元素和类组件指定的特殊属性不同，对常规组件道具没有限制`inputRef`。

这种模式的另一个好处是它可以深入地处理多个组件。例如，假设`Parent`不需要该DOM节点，但是呈现的组件`Parent`（让我们称之为`Grandparent`）需要访问它。然后我们可以让`Grandparent`指定的`inputRef`道具`Parent`，并`Parent`“转发”到`CustomTextInput`：

```javascript
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

function Parent(props) {
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}


class Grandparent extends React.Component {
  render() {
    return (
      <Parent
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

这里，ref回调首先由指定`Grandparent`。它被传递给`Parent`作为一个常规道具称为`inputRef`，`Parent`并将其`CustomTextInput`作为道具传递给它。最后，`CustomTextInput`读取`inputRef`prop并将传递的函数作为`ref`属性附加到`<input>`。其结果是，`this.inputElement`在`Grandparent`将被设置为对应于所述DOM节点`<input>`中的元素`CustomTextInput`。

所有事情都考虑到了，我们建议尽可能暴露DOM节点，但这可能是一个有用的逃生孵化器。请注意，这种方法需要您向子组件添加一些代码。如果你完全不能控制子组件的实现，你最后的选择是使用`findDOMNode()`，但不鼓励。

### 旧版API：字符串参考

如果你之前使用过React，那么你可能会熟悉一个较老的API，其中`ref`属性是一个字符串`"textInput"`，并且DOM节点被访问为`this.refs.textInput`。我们建议不要这样做，因为字符串引用有[一些问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)，被认为是遗留[问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)，并且**可能会在将来的某个版本中删除**。如果您当前正在使用`this.refs.textInput`访问参考，我们建议使用回调模式。

### 注意事项

如果`ref`回调被定义为一个内联函数，它将在更新期间被调用两次，首先`null`是DOM元素，然后再一次。这是因为每个渲染都会创建一个新的函数实例，所以React需要清除旧的参考并设置新的实例。您可以通过将`ref`回调定义为该类的绑定方法来避免这种情况，但请注意，在大多数情况下它不应该存在问题。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18