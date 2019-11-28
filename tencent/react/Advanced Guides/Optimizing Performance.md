# Optimizing Performance

在内部，React 使用几种聪明的技术来最小化更新 UI 所需的昂贵 DOM 操作的数量。对于许多应用程序而言，使用 React 将导致快速的用户界面，而无需专门针对性能进行优化。不过，有几种方法可以加速您的 React 应用程序。

## 使用生产构建

如果您在 React 应用程序中进行基准测试或遇到性能问题，请确保您正在使用缩小的生产版本进行测试。

默认情况下，React 包含许多有用的警告。这些警告在开发中非常有用。但是，他们会使React变得越来越大，所以您应该确保在部署应用程序时使用生产版本。

如果您不确定您的构建过程是否设置正确，可以通过安装**适用于 Chrome 的 React Developer Tools 进行**检查。如果您在生产模式下使用 React 访问网站，该图标将具有黑色背景：

![img](https://ask.qcloudimg.com/http-save/devdocs/k1em5dmjz2.png)

如果您以开发模式访问 React 网站，该图标将具有红色背景：

![img](https://ask.qcloudimg.com/http-save/devdocs/8v0cg2vi2f.png)

预计在使用应用程序时使用开发模式，在将应用程序部署到用户时使用生产模式。

您可以在下面找到有关为您的应用生成应用的说明。

### 创建 React App

如果您的项目是使用 **Create React App** 构建的，请运行：

```javascript
npm run build
```

这将在`build/`您的项目文件夹中创建您的应用程序的生产版本。

请记住，这只是在部署到生产之前需要的。对于正常的开发，使用`npm start`。

### 单文件构建

我们提供 React 和 React DOM 的生产就绪版本作为单个文件：

```javascript
<script src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

请记住，只有以结尾的 React 文件`.production.min.js`适用于生产。

### 分支

为了最有效的 Brunch 生产版本，安装`**uglify-js-brunch**`插件：

```javascript
# If you use npm
npm install --save-dev uglify-js-brunch

# If you use Yarn
yarn add --dev uglify-js-brunch
```

然后，要创建生产版本，请将`-p`标志添加到`build`命令中：

```javascript
brunch build -p
```

请记住，您只需要为生产构建执行此操作。你不应该`-p`在开发中通过标志或应用这个插件，因为它会隐藏有用的 React 警告，并且使构建慢得多。

### Browserify

对于最高效的 Browserify 生产版本，安装一些插件：

```javascript
# If you use npm
npm install --save-dev envify uglify-js uglifyify 

# If you use Yarn
yarn add --dev envify uglify-js uglifyify 
```

要创建生产版本，请确保您添加这些转换**（顺序很重要）**：

- `**envify**`变换确保正确的编译环境设置。使其成为全球（`-g`）。

- `**uglifyify**`转换消除了开发导入。使它成为全局（`-g`）。

- 最后，产生的束被传送到束缚`**uglify-js**`**（阅读为什么）**。

例如：

```javascript
browserify ./index.js \
  -g [ envify --NODE_ENV production ] \
  -g uglifyify \
  | uglifyjs --compress --mangle > ./bundle.js
```

> **注意：** 包名称是`uglify-js`，但它提供的二进制文件被调用`uglifyjs`。这不是一个错字。

请记住，您只需要为生产构建执行此操作。你不应该在开发中应用这些插件，因为它们会隐藏有用的 React 警告，并使构建更慢。

### 卷起

要获得最高效的汇总生产版本，请安装一些插件：

```javascript
# If you use npm
npm install --save-dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify 

# If you use Yarn
yarn add --dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify 
```

要创建生产版本，请确保您添加这些插件**（该订单很重要）**：

- `**replace**`插件确保设置正确的构建环境。

- `**commonjs**`插件提供对 Rollup 中 CommonJS 的支持。

- `**uglify**`插件压缩和轧液最终束。

```javascript
plugins: [
  // ...
  require('rollup-plugin-replace')({
    'process.env.NODE_ENV': JSON.stringify('production')
  }),
  require('rollup-plugin-commonjs')(),
  require('rollup-plugin-uglify')(),
  // ...
]
```

有关完整的设置示例，**请参阅此要点**。

请记住，您只需要为生产构建执行此操作。您不应该在开发中将`uglify`插件或`replace`插件应用于`'production'`值，因为它们会隐藏有用的 React 警告，并且使构建更慢。

### WebPack

> **注意：** 如果您使用 Create React App，请按照上述说明操作。本节仅与直接配置 webpack 有关。

要获得最高效的 webpack 生产版本，请确保将这些插件包含在您的生产配置中：

```javascript
new webpack.DefinePlugin({
  'process.env.NODE_ENV': JSON.stringify('production')
}),
new webpack.optimize.UglifyJsPlugin()
```

你可以在 **webpack 文档中**了解更多。

请记住，您只需要为生产构建执行此操作。你不应该在开发中应用`UglifyJsPlugin`或者`DefinePlugin`具有`'production'`价值，因为它们会隐藏有用的React警告，并且使构建变得更慢。

## 使用 Chrome 性能标签分析组件

在**开发**模式中，您可以使用受支持的浏览器中的性能工具来可视化组件的安装，更新和卸载方式。例如：

![img](https://ask.qcloudimg.com/http-save/devdocs/jac1phps9w.png)

在 Chrome 中执行此操作：

1. 在查询字符串中`?react_perf`加载您的应用程序（例如，`http://localhost:3000/?react_perf`）。

1. 打开 Chrome DevTools **性能**选项卡并按下**记录**。

1. 执行你想要分析的动作。记录时间不要超过20秒，否则 Chrome 可能会挂起。

1. 停止录制。

\5. 反应事件将被分组在**User Timing**标签下。

请注意，这些数字是相对的，因此组件在生产中会呈现更快速度 不过，这应该有助于您意识到不相关的用户界面被错误更新的时间，以及用户界面更新发生的频率和频率。

目前Chrome，Edge和IE是唯一支持此功能的浏览器，但我们使用标准的[User Timing API，](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API)因此我们希望更多浏览器为其增加支持。

## 避免调节

React构建并维护呈现的UI的内部表示。它包含从组件返回的React元素。这种表示让React避免了创建DOM节点和访问现有的节点，因为这可能比JavaScript对象上的操作慢。有时它被称为“虚拟DOM”，但它在React Native上的工作方式相同。

当组件的道具或状态发生变化时，React通过比较新返回的元素和先前渲染的元素来决定是否需要实际的DOM更新。当它们不相等时，React将更新DOM。

在某些情况下，您的组件可以通过重写生命周期函数来加速所有这些，生命周期函数`shouldComponentUpdate`在重新呈现过程开始之前触发。该函数的默认实现返回`true`，使React执行更新：

```javascript
shouldComponentUpdate(nextProps, nextState) {
  return true;
}
```

如果你知道，在某些情况下，你的组件并不需要更新，您可以返回`false`从`shouldComponentUpdate`而是跳过整个渲染过程，包括调用`render()`此组件和下方。

## **shouldComponentUpdate In Action**

这是一个组件的子树。对于每一个，`SCU`指示`shouldComponentUpdate`返回的内容，并`vDOMEq`指出呈现的React元素是否相同。最后，圆圈的颜色表示组件是否需要调和。

![img](https://doc.ebichu.cc/react/img/docs/should-component-update.png)

由于以C2为基础的子树`shouldComponentUpdate`返回`false`，因此React不会尝试渲染C2，因此甚至不必`shouldComponentUpdate`在C4和C5上调用。

对于C1和C3，`shouldComponentUpdate`返回`true`，所以React必须下到叶子并检查它们。对于`shouldComponentUpdate`返回的C6 `true`，由于渲染的元素不相同，React必须更新DOM。

最后一个有趣的案例是C8。React必须渲染这个组件，但是由于它返回的React元素与之前渲染的元素相同，所以它不必更新DOM。

请注意，React只需对C6进行DOM突变，这是不可避免的。对于C8，它通过比较渲染的React元素和C2的子树和C7来保护，甚至不需要比较我们保存的元素`shouldComponentUpdate`，`render`也没有调用。

## **示例**

如果你的组件改变的唯一方式是当变量`props.color`或`state.count`变量发生变化时，你可以`shouldComponentUpdate`检查：

```javascript
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    <button
      color={this.props.color}
      onClick={() => this.setState(state => ({count: state.count + 1}))}>
      Count: {this.state.count}
    </button>
  }
}
```

在这段代码，`shouldComponentUpdate`只是检查是否有任何变化`props.color`或`state.count`。如果这些值不会更改，则组件不会更新。如果你的组件变得更加复杂，你可以使用类似的模式在组件的所有字段之间进行“浅层比较” `props`，`state`以确定组件是否应该更新。这种模式很普遍，React提供了一个帮助器来使用这个逻辑 - 只是从中继承而来的`React.PureComponent`。所以这段代码是一个简单的方法来实现同样的事情：

```javascript
class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    <button
      color={this.props.color}
      onClick={() => this.setState(state => ({count: state.count + 1}))}>
      Count: {this.state.count}
    </button>
  }
}
```

大多数时候，你可以使用`React.PureComponent`而不是写自己的`shouldComponentUpdate`。它只是进行浅层比较，所以如果道具或状态可能以浅层比较错过的方式进行了变异，则不能使用它。

这可能是一个更复杂的数据结构的问题。例如，假设您希望`ListOfWords`组件呈现以逗号分隔的单词列表，并使用父`WordAdder`组件通过单击按钮将单词添加到列表中。此代码*无法*正常工作：

```javascript
class ListOfWords extends React.PureComponent {
  render() {
    return <div>{this.props.words.join(',')}</div>;
  }
}

class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['marklar']
    };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // This section is bad style and causes a bug
    const words = this.state.words;
    words.push('marklar');
    this.setState({words: words});
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick} />
        <ListOfWords words={this.state.words} />
      </div>
    );
  }
}
```

问题是`PureComponent`将会对旧的和新的值进行简单的比较`this.props.words`。由于这段代码`words`在`handleClick`方法中改变了数组，所以即使数组中的实际字已经改变`WordAdder`，旧的和新的值`this.props.words`也会相等。该`ListOfWords`因此将不再更新，即使它有768,16呈现新词。

## **不突变数据的力量**

避免此问题的最简单方法是避免将您正在用作道具或状态的值进行变异。例如，`handleClick`上面的方法可以使用`concat`as 重写：

```javascript
handleClick() {
  this.setState(prevState => ({
    words: prevState.words.concat(['marklar'])
  }));
}
```

ES6支持数组的[扩展语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)，这可以使这更容易。如果您正在使用Create React App，则默认情况下此语法可用。

```javascript
handleClick() {
  this.setState(prevState => ({
    words: [...prevState.words, 'marklar'],
  }));
};
```

您也可以以类似的方式重写改变对象以避免突变的代码。例如，假设我们有一个名为object的对象，`colormap`并且我们想要编写一个更改`colormap.right`为的函数`'blue'`。我们可以写：

```javascript
function updateColorMap(colormap) {
  colormap.right = 'blue';
}
```

要写这个而不改变原始对象，我们可以使用[Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)方法：

```javascript
function updateColorMap(colormap) {
  return Object.assign({}, colormap, {right: 'blue'});
}
```

`updateColorMap`现在返回一个新对象，而不是改变旧对象。`Object.assign`在ES6中，需要填充。

JavaScript提议添加[对象传播属性](https://github.com/sebmarkbage/ecmascript-rest-spread)，以便更容易更新对象而无需进行突变：

```javascript
function updateColorMap(colormap) {
  return {...colormap, right: 'blue'};
}
```

如果您使用的是创建React应用程序，`Object.assign`则默认情况下都可以使用对象传播语法。

## **使用不可变数据结构**

[Immutable.js](https://github.com/facebook/immutable-js)是解决这个问题的另一种方法。它提供了通过结构共享工作的不变的，持久的集合：

- *Immutable*：一旦创建，一个集合不能在另一个时间点被修改。

- *Persistent*：可以根据以前的集合和诸如集合之类的变体创建新集合。新集合创建后，原始集合仍然有效。

- *Structural Sharing*：使用与原始集合尽可能多的相同结构创建新集合，尽量减少复制以提高性能。

不变性使追踪变化便宜。更改总是会导致一个新的对象，所以我们只需要检查对象的引用是否已经改变。例如，在这个常规的 JavaScript 代码中：

```javascript
const x = { foo: 'bar' };
const y = x;
y.foo = 'baz';
x === y; // true
```

虽然`y`是编辑过的，但由于它是对同一个对象的引用，所以`x`此比较返回`true`。你可以用 immutable.js 编写类似的代码：

```javascript
const SomeRecord = Immutable.Record({ foo: null });
const x = new SomeRecord({ foo: 'bar' });
const y = x.set('foo', 'baz');
const z = x.set('foo', 'bar');
x === y; // false
x === z; // true
```

在这种情况下，由于在变异时返回一个新的引用`x`，所以我们可以使用引用相等性检查`(x === y)`来验证存储的新值与存储在`y`其中的原始值不同`x`。

另外两个可以帮助使用不可变数据的库是**无缝不可变和不可变的帮助器**。

不可变的数据结构为您提供了一种便捷的方式来跟踪对象的变化，这是我们需要实现的`shouldComponentUpdate`。这通常可以为你提供很好的性能提升。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18