# Forms

HTML表单元素与React中的其他DOM元素有点不同，因为表单元素自然会保留一些内部状态。例如，这个纯HTML格式的表单接受一个名称：

```javascript
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

此表单具有用户提交表单时浏览到新页面的默认HTML表单行为。如果你想在React中使用这种行为，那就行了。但是在大多数情况下，使用JavaScript函数可以方便地处理提交表单并可以访问用户在表单中输入的数据。实现这一目标的标准方法是使用称为“受控组件”的技术。

## 受控组件

在HTML中，表单元素（例如`<input>`，`<textarea>`）`<select>`通常保持其自己的状态并根据用户输入进行更新。在React中，可变状态通常保存在组件的状态属性中，并且只能使用更新`setState()`。

我们可以通过使React状态成为“单一真相源”来结合这两者。然后，呈现表单的React组件也控制后续用户输入中以该表单发生的事情。输入表单元素的值由React以这种方式控制，称为“受控组件”。

例如，如果我们想让前面的示例在提交时记录名称，我们可以将表单编写为受控组件：

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

由于`value`属性设置在我们的表单元素上，因此显示的值将始终为`this.state.value`，使React状态成为真值的来源。由于`handleChange`每次击键时都会运行以更新React状态，因此显示的值将随用户键入而更新。

对于受控组件，每个状态变异都会有一个关联的处理函数。这使修改或验证用户输入变得非常简单。例如，如果我们想强制使用全部大写字母来编写名称，我们可以这样写`handleChange`：

```javascript
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

## textarea标签

在HTML中，一个`<textarea>`元素通过子元素定义其文本：

```javascript
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

在React中， 改为`<textarea>`使用`value`属性。这样，使用a的表单`<textarea>`可以非常类似于使用单行输入的表单：

```javascript
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

注意`this.state.value`在构造函数中初始化，以便文本区域以其中的一些文本开始。

## 选择标签

在HTML中，`<select>`创建一个下拉列表。例如，这个HTML创建一个下拉列表：

```javascript
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

请注意，由于`selected`属性，Coconut选项最初被选中。反应，而不是使用此`selected`属性，使用`value`根`select`标签上的属性。这在受控组件中更方便，因为您只需要在一个位置更新它。例如：

```javascript
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

总的来说，这使得它，这样`<input type="text">`，`<textarea>`和`<select>`所有的工作非常相似-他们都接受`value`，你可以用它来实现控制组件属性。

> 注意您可以将数组传递给该`value`属性，从而允许您在`select`标签中选择多个选项：<select multiple = {true} value = {'B'，'C'}>

## 处理多个输入

当需要处理多个受控`input`元素时，可以`name`为每个元素添加一个属性，并让处理函数根据值来选择要执行的操作`event.target.name`。

例如：

```javascript
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

请注意，我们如何使用ES6 [计算的属性名称](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names)语法来更新与给定输入名称对应的状态键：

```javascript
this.setState({
  [name]: value
});
```

它相当于这个ES5代码：

```javascript
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

此外，由于`setState()`自动将局部状态合并到当前状态，因此我们只需要用改变的部分调用它。

## 受控输入空值

在受控组件上指定值prop可以防止用户更改输入，除非您愿意。如果您已指定a `value`但输入仍可编辑，则可能意外设置`value`为`undefined`或`null`。

以下代码演示了这一点。（输入首先被锁定，但在短暂延迟后变为可编辑。）

```javascript
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```

## 受控组件的替代品

使用受控组件有时会非常乏味，因为您需要为数据可以改变的每种方式编写一个事件处理程序，并通过React组件管理所有输入状态。当您将预先存在的代码库转换为React或将React应用程序与非React库集成时，这会变得特别烦人。在这些情况下，您可能需要检查不受控制的组件，这是实现输入表单的另一种技术。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18