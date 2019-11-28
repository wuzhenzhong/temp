# Uncontrolled Components

在大多数情况下，我们建议使用受控组件来实现表单。在受控组件中，表单数据由React组件处理。另一种选择是不受控制的组件，其中表单数据由DOM本身处理。

要编写一个不受控制的组件，而不是为每个状态更新编写一个事件处理程序，可以使用ref从DOM获取表单值。

例如，该代码在不受控制的组件中接受单个名称：

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在CodePen上试用它。](https://codepen.io/gaearon/pen/WooRWa?editors=0010)

由于不受控制的组件在DOM中保留了真相源，因此在使用不受控制的组件时，集成React和非React代码有时更容易。如果你想快速和肮脏，它也可以少一点代码。否则，您通常应该使用受控组件。

如果仍然不清楚您应该在特定情况下使用哪种类型的组件，那么您可能会发现[这篇关于受控与非受控输入的文章](http://goshakkk.name/controlled-vs-uncontrolled-inputs-react/)会有所帮助。

### 默认值

在React渲染生命周期中，`value`表单元素上的属性将覆盖DOM中的值。对于不受控制的组件，您通常希望React指定初始值，但让后续更新不受控制。要处理这种情况，您可以指定一个`defaultValue`属性而不是`value`。

```javascript
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={(input) => this.input = input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

同样，`<input type="checkbox">`和`<input type="radio">`支持`defaultChecked`，并`<select>`与`<textarea>`支持`defaultValue`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18