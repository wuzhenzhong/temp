# Lifting State Up

通常，几个组件需要反映相同的变化数据。我们建议将共享状态提升至最接近的共同祖先。让我们看看这是如何运作的。

在本节中，我们将创建一个温度计算器，用于计算在给定温度下水是否沸腾。

我们将从一个叫做组件开始`BoilingVerdict`。它接受`celsius`温度作为支柱，并打印是否足够煮沸水：

```javascript
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

接下来，我们将创建一个名为的组件`Calculator`。它呈现一个`<input>`让你输入温度，并保持其价值`this.state.temperature`。

此外，它呈现`BoilingVerdict`当前输入值。

```javascript
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```

**在 CodePen 上试用它。**

## 添加第二个输入

我们的新要求是，除了摄氏温度输入外，我们还提供华氏温度输入，并且它们保持同步。

我们可以从提取`TemperatureInput`组件开始`Calculator`。我们将添加一个新的`scale`道具，它可以是`"c"`或者`"f"`：

```javascript
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

我们现在可以改变`Calculator`以呈现两个单独的温度输入：

```javascript
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

**在 CodePen 上试用它。**

我们现在有两个输入，但是当你在其中一个输入温度时，另一个不会更新。这与我们的要求相矛盾：我们希望保持同步。

我们也无法显示`BoilingVerdict`从 `Calculator`。在`Calculator`不知道当前的温度，因为它是藏在里面的`TemperatureInput`。

## 编写转换函数

首先，我们将编写两个函数将摄氏温度转换为华氏温度，然后返回：

```javascript
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

这两个函数转换数字。我们将编写另一个函数，它将字符串`temperature`和转换器函数作为参数并返回一个字符串。我们将使用它来计算基于其他输入的一个输入的值。

它会在无效的情况下返回一个空字符串`temperature`，并将输出保留为小数点后第三位：

```javascript
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

例如，`tryConvert('abc', toCelsius)`返回一个空字符串，并`tryConvert('10.22', toFahrenheit)`返回`'50.396'`。

## 提升状态

目前，这两个`TemperatureInput`组件都独立地将其值保持在本地状态：

```javascript
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    // ...  
```

但是，我们希望这两个输入互相同步。当我们更新摄氏温度输入时，华氏温度输入应反映转换后的温度，反之亦然。

在 React 中，共享状态是通过将它移动到需要它的组件的最接近的共同祖先来完成的。这被称为“提升状态”。我们将从当地国家中移除`TemperatureInput`并将其移入`Calculator`。

如果`Calculator`拥有共享状态，它将成为两个输入中当前温度的“真值源”。它可以指导他们都有相互一致的价值观。由于两个`TemperatureInput`组件的道具来自同一个父`Calculator`组件，因此两个输入始终保持同步。

让我们看看这是如何一步一步工作。

首先，我们将替换`this.state.temperature`用`this.props.temperature`的`TemperatureInput`部件。现在，让我们假装`this.props.temperature`已经存在，尽管我们需要`Calculator`在未来将它传递出去：

```javascript
  render() {
    // Before: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
    // ...
```

我们知道道具是只读的。当`temperature`在当地的状态，`TemperatureInput`可以打电话`this.setState()`来改变它。但是，现在`temperature`来自父母的道具，`TemperatureInput`它无法控制它。

在 React 中，通常通过将组件“控制”来解决这个问题。就像 DOM `<input>`接受a `value`和`onChange`prop一样，自定义也可以`TemperatureInput`接受它的父项`temperature`和`onTemperatureChange`道具`Calculator`。

现在，当`TemperatureInput`想要更新其温度时，它会调用`this.props.onTemperatureChange`：

```javascript
  handleChange(e) {
    // Before: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
    // ...
```

> 注意：对自定义组件中的任一`temperature`或`onTemperatureChange`名称没有特殊含义。我们可以称其他任何东西，比如说它们的名字`value`，`onChange`这是一个通用的惯例。

`onTemperatureChange`支柱将与一起提供`temperature`由父支柱`Calculator`组件。它将通过修改其自身的本地状态来处理更改，从而使用新值重新呈现两个输入。我们`Calculator`很快就会看到新的实施。

在深入了解变化之前`Calculator`，让我们回顾一下对`TemperatureInput`组件的更改。我们已经从中删除了当地的国家，而不是阅读`this.state.temperature`，我们现在阅读`this.props.temperature`。`this.setState()`我们现在打电话给我们`this.props.onTemperatureChange()`，而不是打电话给我们，这将由以下人员提供`Calculator`：

```javascript
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

现在我们来看看这个`Calculator`组件。

我们将存储当前的输入`temperature`并`scale`处于本地状态。这是我们从投入中“提起来”的状态，它将成为两者的“真相之源”。它是我们为了呈现两个输入而需要知道的所有数据的最小表示。

例如，如果我们在摄氏度输入中输入37，则`Calculator`组件的状态将为：

```javascript
{
  temperature: '37',
  scale: 'c'
}
```

如果我们稍后编辑华氏场为212，那么`Calculator`将会是：

```javascript
{
  temperature: '212',
  scale: 'f'
}
```

我们可以存储两个输入的值，但事实证明这是不必要的。存储最近更改的输入的值以及它所表示的比例就足够了。然后，我们可以基于当前`temperature`和`scale`单独推断另一个输入的值。

输入保持同步，因为它们的值是从相同的状态计算得出的：

```javascript
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```

**在 CodePen 上试用它。**

现在，无论您编辑哪个输入，`this.state.temperature`和`this.state.scale`在`Calculator`获取更新。其中一个输入按原样得到值，因此任何用户输入都会保留，而另一个输入值总是基于此重新计算。

让我们回顾一下编辑输入时会发生的情况：

- React 调用`onChange`在 DOM 上指定的函数`<input>`。在我们的例子中，这是组件中的`handleChange`方法`TemperatureInput`。

- 组件中的`handleChange`方法使用新的期望值`TemperatureInput`调用`this.props.onTemperatureChange()`。它的道具，其中包括`onTemperatureChange`，由其母公司提供的`Calculator`。

- 当它先前渲染中，`Calculator`已经指定`onTemperatureChange`了摄氏的`TemperatureInput`是`Calculator`的`handleCelsiusChange`方法，以及`onTemperatureChange`所述华氏`TemperatureInput`是`Calculator`的`handleFahrenheitChange`方法。所以`Calculator`根据我们编辑的输入来调用这两个方法。

- 在这些方法中，`Calculator`组件要求 React 通过调用`this.setState()`新的输入值和我们刚刚编辑的输入的当前比例重新呈现自己。

- React 调用`Calculator`组件的`render`方法来了解 UI 的外观。根据当前温度和活动比例重新计算两个输入的值。温度转换在这里执行。

- React 用它们指定的新道具调用`render`各个`TemperatureInput`组件的方法`Calculator`。它了解他们的用户界面应该是什么样子。

- React DOM 更新 DOM 以匹配所需的输入值。我们刚刚编辑的输入接收其当前值，另一个输入更新为转换后的温度。

每次更新都经历相同的步骤，以使输入保持同步。

## 得到教训

对于在 React 应用程序中更改的任何数据，应该有一个“真相源”。通常，首先将状态添加到需要渲染的组件中。然后，如果其他组件也需要它，可以将它提升到最接近的共同祖先。与其试图在不同组件之间同步状态，您应该依赖自顶向下的数据流。

提升状态涉及编写比双向绑定方法更多的“样板”代码，但作为一个好处，查找和隔离错误需要较少的工作。由于任何状态“存在于”某个组件中，并且该组件本身可以改变它，所以错误的表面积大大降低。另外，您可以实现任何自定义逻辑来拒绝或转换用户输入。

如果某件事可以从道具或状态中推导出来，那么它可能不应该处于这个状态。例如，而不是存储既`celsiusValue`和`fahrenheitValue`，我们只是存储上次编辑`temperature`和`scale`。其他输入的值可以始终由`render()`方法中的值来计算。这让我们可以清除或应用舍入到其他字段，而不会丢失用户输入的任何精度。

当您在 UI 中发现错误时，可以使用 **React Developer Tools** 检查道具并向上移动树，直到找到负责更新状态的组件。这可以让你追踪错误来源：

![img](https://d33wubrfki0l68.cloudfront.net/05ade6c3d5ae581cb542d01ac6980aaf703763c5/2f815/ef94afc3447d75cdc245c77efb0d63be.gif)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com