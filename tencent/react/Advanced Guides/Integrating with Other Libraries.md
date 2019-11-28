# Integrating with Other Libraries

React可以用于任何Web应用程序。它可以嵌入到其他应用程序中，并且可以将其他应用程序嵌入到React中。本指南将检查一些更常见的用例，重点介绍与[jQuery](https://jquery.com/)和[Backbone的](http://backbonejs.org/)集成，但是可以将相同的思想应用于将组件与任何现有代码集成。

## 与DOM操作插件集成

[纠错](javascript:;)

React不知道在React之外对DOM做出的更改。它根据自己的内部表示来确定更新，并且如果相同的DOM节点被另一个库操纵，则React会感到困惑并且无法恢复。

这并不意味着将React与其他影响DOM的方式结合起来是不可能的，甚至是一定困难的，您只需要注意每个人正在做什么。

避免冲突的最简单方法是防止更新React组件。你可以通过渲染React没有理由更新的元素来做到这一点，比如空白`<div />`。

### 如何解决这个问题

为了演示这一点，让我们勾勒一个通用jQuery插件的包装。

我们将附加一个ref到根DOM元素。在里面`componentDidMount`，我们会得到一个引用，所以我们可以将它传递给jQuery插件。

为了防止触摸安装后的DOM反应，我们会返回一个空`<div />`从`render()`方法。该`<div />`元素没有属性或子元素，因此React没有理由更新它，让jQuery插件可以自由地管理DOM的这一部分：

```javascript
class SomePlugin extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.somePlugin();
  }

  componentWillUnmount() {
    this.$el.somePlugin('destroy');
  }

  render() {
    return <div ref={el => this.el = el} />;
  }
}
```

请注意，我们定义了两个`componentDidMount`和`componentWillUnmount`生命周期钩子。许多jQuery插件将事件监听器附加到DOM，因此将它们分开是非常重要的`componentWillUnmount`。如果插件没有提供清理方法，则可能需要提供自己的插件，记住删除插件注册的任何事件侦听器，以防止内存泄漏。

### 与jQuery选择插件集成

对于这些概念的更具体的例子，我们来为插件[Chosen](https://harvesthq.github.io/chosen/)写一个最小包装，它增加了`<select>`输入。

> **注意：** 仅仅因为这可能，并不意味着它是React应用程序的最佳方法。我们建议您尽可能使用React组件。React组件更容易在React应用程序中重用，并且通常可以更好地控制其行为和外观。

首先，我们来看看选择DOM对于什么。

如果您在`<select>`DOM节点上调用它，它会从原始DOM节点读取属性，将其隐藏为内联样式，然后在其后面附加一个单独的DOM节点，并在其后面附带自己的可视化表示`<select>`。然后它会触发jQuery事件来通知我们有关更改。

假设这是我们用`<Chosen>`包装器React组件争取的API ：

```javascript
function Example() {
  return (
    <Chosen onChange={value => console.log(value)}>
      <option>vanilla</option>
      <option>chocolate</option>
      <option>strawberry</option>
    </Chosen>
  );
}
```

我们将把它作为一个不受控制的组件来简化。

首先，我们将创建一个空的成分`render()`，我们返回方法`<select>`裹着`<div>`：

```javascript
class Chosen extends React.Component {
  render() {
    return (
      <div>
        <select className="Chosen-select" ref={el => this.el = el}>
          {this.props.children}
        </select>
      </div>
    );
  }
}
```

注意我们`<select>`是如何包裹在一个额外的`<div>`。这是必要的，因为Chosen会在`<select>`我们传递给它的节点之后追加另一个DOM元素。但是，就React而言，`<div>`总是只有一个孩子。这就是我们如何确保React更新不会与由Chosen附加的额外DOM节点发生冲突的原因。重要的是，如果您在React流的外部修改DOM，则必须确保React没有理由触摸这些DOM节点。

接下来，我们将实现生命周期挂钩。我们需要初始化选中的参考`<select>`节点`componentDidMount`，并将其拆分为`componentWillUnmount`：

```javascript
componentDidMount() {
  this.$el = $(this.el);
  this.$el.chosen();
}

componentWillUnmount() {
  this.$el.chosen('destroy');
}
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/qmqeQx?editors=0010)

请注意，React不会为该`this.el`字段赋予特殊含义。它只能工作，因为我们以前`ref`在该`render()`方法中从a分配了该字段：

```javascript
<select className="Chosen-select" ref={el => this.el = el}>
```

这足以让我们的组件渲染，但我们也希望得到关于值更改的通知。为此，我们将订阅由Chosen管理的jQuery `change`事件`<select>`。

我们不会`this.props.onChange`直接通过选择，因为组件的道具可能随时间而改变，并且包括事件处理程序。相反，我们将声明一个`handleChange()`调用方法`this.props.onChange`，并将其订阅到jQuery `change`事件中：

```javascript
componentDidMount() {
  this.$el = $(this.el);
  this.$el.chosen();

  this.handleChange = this.handleChange.bind(this);
  this.$el.on('change', this.handleChange);
}

componentWillUnmount() {
  this.$el.off('change', this.handleChange);
  this.$el.chosen('destroy');
}

handleChange(e) {
  this.props.onChange(e.target.value);
}
```

[在CodePen上试用。](http://codepen.io/gaearon/pen/bWgbeE?editors=0010)

最后还有一件事要做。在React中，道具可以随时间变化。例如，`<Chosen>`如果父组件的状态更改，组件可以获得不同的子项。这意味着在集成点，我们手动更新DOM以响应prop更新非常重要，因为我们不再让React为我们管理DOM。

Chosen的文档建议我们可以使用jQuery `trigger()`API来通知它对原始DOM元素的更改。我们将让React负责`this.props.children`内部更新`<select>`，但我们还将添加一个`componentDidUpdate()`生命周期挂钩，通知Chosen关于子列表中的更改：

```javascript
componentDidUpdate(prevProps) {
  if (prevProps.children !== this.props.children) {
    this.$el.trigger("chosen:updated");
  }
}
```

这样，当`<select>`由React管理的孩子发生变化时，Chosen会知道更新其DOM元素。

`Chosen`组件的完整实现如下所示：

```javascript
class Chosen extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.chosen();

    this.handleChange = this.handleChange.bind(this);
    this.$el.on('change', this.handleChange);
  }
  
  componentDidUpdate(prevProps) {
    if (prevProps.children !== this.props.children) {
      this.$el.trigger("chosen:updated");
    }
  }

  componentWillUnmount() {
    this.$el.off('change', this.handleChange);
    this.$el.chosen('destroy');
  }
  
  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    return (
      <div>
        <select className="Chosen-select" ref={el => this.el = el}>
          {this.props.children}
        </select>
      </div>
    );
  }
}
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/xdgKOz?editors=0010)

## 与其他视图库集成

由于灵活性，React可以嵌入到其他应用程序中`ReactDOM.render()`。

虽然React在启动时通常用于将单个根React组件加载到DOM中，`ReactDOM.render()`但也可以为UI的独立部分多次调用，该部分可以像按钮一样小，也可以与应用程序一样大。

事实上，这正是Facebook如何使用React。这让我们可以在React中逐个编写应用程序，并将其与我们现有的服务器生成的模板和其他客户端代码结合使用。

### 用React替换基于字符串的渲染

旧Web应用程序中的一种常见模式是将DOM的块描述为字符串，并将其插入到DOM中，如下所示：`$el.html(htmlString)`。代码库中的这些点非常适合引入React。只需将基于字符串的渲染重写为React组件即可。

所以下面的jQuery实现...

```javascript
$('#container').html('<button id="btn">Say Hello</button>');
$('#btn').click(function() {
  alert('Hello!');
});
```

...可以使用React组件重写：

```javascript
function Button() {
  return <button id="btn">Say Hello</button>;
}

ReactDOM.render(
  <Button />,
  document.getElementById('container'),
  function() {
    $('#btn').click(function() {
      alert('Hello!');
    });
  }
);
```

从这里开始，您可以开始将更多逻辑转移到组件中，并开始采用更常见的React实践。例如，在组件中，最好不要依赖ID，因为可以多次渲染相同的组件。相反，我们将使用React事件系统，并将点击处理程序直接注册到React `<button>`元素上：

```javascript
function Button(props) {
  return <button onClick={props.onClick}>Say Hello</button>;
}

function HelloButton() {
  function handleClick() {
    alert('Hello!');
  }
  return <Button onClick={handleClick} />;
}

ReactDOM.render(
  <HelloButton />,
  document.getElementById('container')
);
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/RVKbvW?editors=1010)

您可以拥有任意数量的此类隔离组件，并使用`ReactDOM.render()`它们将它们呈现给不同的DOM容器。逐渐地，当您将更多应用程序转换为React时，您将能够将它们组合成更大的组件，并将一些`ReactDOM.render()`调用移动到层次结构中。

### 在骨干视图中嵌入React

[主干](http://backbonejs.org/)视图通常使用HTML字符串或字符串生成模板函数来为其DOM元素创建内容。这个过程也可以用渲染React组件来替换。

下面，我们将创建一个名为Backbone的视图`ParagraphView`。它将覆盖Backbone的`render()`函数，将React `<Paragraph>`组件渲染到由Backbone（`this.el`）提供的DOM元素中。在这里，我们也在使用`ReactDOM.render()`：

```javascript
function Paragraph(props) {
  return <p>{props.text}</p>;
}

const ParagraphView = Backbone.View.extend({
  render() {
    const text = this.model.get('text');
    ReactDOM.render(<Paragraph text={text} />, this.el);
    return this;
  },
  remove() {
    ReactDOM.unmountComponentAtNode(this.el);
    Backbone.View.prototype.remove.call(this);
  }
});
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/gWgOYL?editors=0010)

我们也呼吁是非常重要`ReactDOM.unmountComponentAtNode()`的`remove`方法，以便作出反应注销事件处理程序，并与组件树相关的其他资源，当它被分离。

当一个组件*从*一个React树中被移除时，清理会自动执行，但因为我们要手动移除整个树，所以我们必须把它称为这个方法。

## 与模型层集成

虽然通常建议使用单向数据流，例如React状态，[Flux](http://facebook.github.io/flux/)或[Redux](http://redux.js.org/)，但React组件可以使用其他框架和库中的模型层。

### 在React组件中使用Backbone模型

使用React组件的[Backbone](http://backbonejs.org/)模型和集合的最简单方法是侦听各种更改事件并手动强制更新。

负责渲染模型的组件将监听`'change'`事件，而负责渲染集合的组件将监听`'add'`和`'remove'`事件。在这两种情况下，都需要`this.forceUpdate()`使用新数据调用组件。

在下面的示例中，`List`组件呈现Backbone集合，使用该`Item`组件呈现单个项目。

```javascript
class Item extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.model.on('change', this.handleChange);
  }

  componentWillUnmount() {
    this.props.model.off('change', this.handleChange);
  }

  render() {
    return <li>{this.props.model.get('text')}</li>;
  }
}

class List extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.collection.on('add', 'remove', this.handleChange);
  }

  componentWillUnmount() {
    this.props.collection.off('add', 'remove', this.handleChange);
  }

  render() {
    return (
      <ul>
        {this.props.collection.map(model => (
          <Item key={model.cid} model={model} />
        ))}
      </ul>
    );
  }
}
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/GmrREm?editors=0010)

### 从主干模型中提取数据

上述方法要求您的React组件知道Backbone模型和集合。如果您后来计划迁移到另一个数据管理解决方案，则可能需要尽可能少地将有关Backbone的知识集中在代码中。

解决这个问题的一个办法是在模型的属性发生变化时将模型的属性作为普通数据提取出来，并将这个逻辑放在一个地方。以下是一个高阶组件，它将Backbone模型的所有属性提取到状态中，并将数据传递给包装组件。

这样，只有高阶组件需要了解Backbone模型内部，并且应用程序中的大多数组件都可以不依赖于Backbone。

在下面的例子中，我们将复制模型的属性以形成初始状态。我们订阅`change`事件（并取消订阅卸载），当它发生时，我们用模型的当前属性更新状态。最后，我们确保如果`model`道具本身发生变化，我们不会忘记退订旧模型，并订阅新模型。

请注意，这个例子并不意味着在使用Backbone方面是详尽的，但它应该给你一个关于如何以一种通用的方式来解决这个问题的想法：

```javascript
function connectToBackboneModel(WrappedComponent) {
  return class BackboneComponent extends React.Component {
    constructor(props) {
      super(props);
      this.state = Object.assign({}, props.model.attributes);
      this.handleChange = this.handleChange.bind(this);
    }

    componentDidMount() {
      this.props.model.on('change', this.handleChange);
    }

    componentWillReceiveProps(nextProps) {
      this.setState(Object.assign({}, nextProps.model.attributes));
      if (nextProps.model !== this.props.model) {
        this.props.model.off('change', this.handleChange);
        nextProps.model.on('change', this.handleChange);
      }
    }

    componentWillUnmount() {
      this.props.model.off('change', this.handleChange);
    }

    handleChange(model) {
      this.setState(model.changedAttributes());
    }

    render() {
      const propsExceptModel = Object.assign({}, this.props);
      delete propsExceptModel.model;
      return <WrappedComponent {...propsExceptModel} {...this.state} />;
    }
  }
}
```

为了演示如何使用它，我们将`NameInput`React组件连接到Backbone模型，并在`firstName`每次输入更改时更新其属性：

```javascript
function NameInput(props) {
  return (
    <p>
      <input value={props.firstName} onChange={props.handleChange} />
      <br />
      My name is {props.firstName}.
    </p>
  );
}

const BackboneNameInput = connectToBackboneModel(NameInput);

function Example(props) {
  function handleChange(e) {
    model.set('firstName', e.target.value);
  }

  return (
    <BackboneNameInput
      model={props.model}
      handleChange={handleChange}
    />
  );
}

const model = new Backbone.Model({ firstName: 'Frodo' });
ReactDOM.render(
  <Example model={model} />,
  document.getElementById('root')
);
```

[在CodePen上试用它。](http://codepen.io/gaearon/pen/PmWwwa?editors=0010)

这项技术不限于Backbone。您可以通过订阅其生命周期挂钩中的更改并将数据复制到本地React状态，从而将React用于任何模型库。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18