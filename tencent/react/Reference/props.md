# props

## 单页面应用程序

单页面应用程序是一个加载单个HTML页面和应用程序运行所需的所有必要资源（如JavaScript和CSS）的应用程序。任何与页面或后续页面的交互都不需要往返服务器，这意味着页面不会被重新加载。

虽然您可以在React中构建一个单页面应用程序，但这不是必需的。React还可用于增强现有网站的小部分，并增加交互性。用React编写的代码可以通过PHP或其他客户端库来呈现在服务器上的标记和平共存。事实上，这正是Facebook如何使用React。

[纠错](javascript:;)

## ES6, ES2015, ES2016, etc

这些首字母缩写词都是指ECMAScript语言规范标准的最新版本，这是JavaScript语言的一个实现。ES6版本（也称为ES2015）包含许多以前版本的新增功能，例如：箭头函数，类，模板文字`let`和`const`语句。您可以详细了解具体的版本[在这里](https://en.wikipedia.org/wiki/ECMAScript#Versions)。

## 编译器

JavaScript编译器接受JavaScript代码，将其转换并以不同的格式返回JavaScript代码。最常见的用例是采用ES6语法并将其转换为旧版浏览器能够解释的语法。[Babel](https://babeljs.io/)是React最常用的编译器。

## 捆扎机

Bundlers将JavaScript和CSS代码作为单独的模块（通常为数百个）编写，并将它们组合成更好的浏览器优化的几个文件。React应用程序中常用的一些[打包](https://webpack.js.org/)器包括[Webpack](https://webpack.js.org/)和[Browserify](http://browserify.org/)。

## 包管理员

包管理器是允许您管理项目中的依赖项的工具。[npm](https://www.npmjs.com/)和[Yarn](http://yarnpkg.com/)是React应用程序中常用的两个包管理器。他们都是同一个npm包注册表的客户端。

## CDN

CDN代表内容传送网络。CDN从全球各地的服务器网络提供缓存的静态内容。

## JSX

JSX是JavaScript的语法扩展。它与模板语言类似，但它具有JavaScript的全部功能。JSX被编译为`React.createElement()`调用，它返回名为“React元素”的普通JavaScript对象。要获得对JSX的基本介绍，请参阅此处的文档，并在此处查找关于JSX的更深入的教程。

React DOM使用camelCase属性命名约定而不是HTML属性名称。例如，`tabindex`成为`tabIndex`JSX。该属性`class`也被写为`className`自从`class`在JavaScript中是一个保留字：

```javascript
const name = 'Clementine';
ReactDOM.render(
  <h1 className="hello">My name is {name}!</h1>,
  document.getElementById('root')
);
```

## 分子

React元素是React应用程序的构建块。人们可能会将元素与更广为人知的“组件”概念混为一谈。一个元素描述了你想要在屏幕上看到的内容。React元素是不可变的。

```javascript
const element = <h1>Hello, world</h1>;
```

通常，元素不是直接使用，而是从组件返回。

## 组件

React组件是小的，可重用的代码段，它返回一个React元素以呈现给页面。React组件的最简单版本是一个普通的JavaScript函数，它返回一个React元素：

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

组件也可以是ES6类：

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

组件可以分解成不同的功能块并在其他组件中使用。组件可以返回其他组件，数组，字符串和数字。一个好的经验法则是，如果你的用户界面的一部分被多次使用（按钮，面板，头像），或者它自己已经足够复杂（App，FeedStory，Comment），那么它是一个很好的候选者，可以成为一个可重用的组件。组件名称也应始终以大写字母（`<Wrapper/>` **不** `<wrapper/>`）开头。有关渲染组件的更多信息，请参阅此文档。

### `props`

`props`是React组件的输入。它们是从父组件传递到子组件的数据。

请记住，这`props`是只读的。不应以任何方式对其进行修改：

```javascript
// Wrong!
props.number = 42;
```

如果您需要修改某些值以响应用户输入或网络响应，请`state`改为使用。

### `props.children`

`props.children`在每个组件上都可用。它包含组件的开始和结束标签之间的内容。例如：

```javascript
<Welcome>Hello world!</Welcome>
```

字符串在组件中`Hello world!`可用：`props.childrenWelcome`

```javascript
function Welcome(props) {
  return <p>{props.children}</p>;
}
```

对于定义为类的组件，请使用`this.props.children`：

```javascript
class Welcome extends React.Component {
  render() {
    return <p>{this.props.children}</p>;
  }
}
```

### `state`

`state`当与其关联的某些数据随时间变化时，组件需要。例如，`Checkbox`组件可能需要`isChecked`处于其状态，`NewsFeed`组件可能需要跟踪`fetchedPosts`其状态。

之间最重要的区别`state`和`props`是`props`从父组件传递，而是`state`由组件本身进行管理。一个组件不能改变它`props`，但它可以改变它`state`。要这样做，它必须打电话`this.setState()`。只有定义为类的组件可以具有状态。

对于每一个特定的变化数据，应该只有一个组件“拥有”它的状态。不要尝试同步两个不同组件的状态。相反，将它提升到最接近的共同祖先，并将它作为道具传递给它们两个。

## 生命周期方法

生命周期方法是在组件的不同阶段执行的自定义功能。当组件被创建并插入到DOM中时（安装），组件更新时，组件被卸载或从DOM中移除时，都有可用的方法。

## 受控组件与非受控组件

React有两种不同的方法来处理表单输入。

其值由React控制的输入表单元素称为*受控组件*。当用户将数据输入受控组件时，会触发更改事件处理程序，并且您的代码将决定输入是否有效（通过使用更新的值重新呈现）。如果你不重新渲染，那么表单元素将保持不变。

一个*不受控制的组件*就像表单元素在React之外工作一样。当用户将数据输入到表单域（输入框，下拉菜单等）时，更新后的信息会被反映出来，而不需要React需要做任何事情。然而，这也意味着你不能强迫这个领域有一定的价值。

在大多数情况下，您应该使用受控组件。

## Keys

“键”是在创建元素数组时需要包含的特殊字符串属性。键可帮助React识别哪些项目已更改，添加或删除。应该给数组内的元素赋予元素一个稳定的标识。

键只需要在同一阵列中的同级元素中是唯一的。它们不需要在整个应用程序或甚至单个组件中都是唯一的。

不要传递像`Math.random()`键的东西。密钥在重新呈现中具有“稳定身份”非常重要，以便React可以确定何时添加，删除或重新排序项目。理想情况下，密钥应与来自数据的唯一且稳定的标识符相对应，例如`post.id`。

## Refs

React支持可以附加到任何组件的特殊属性。该`ref`属性可以是字符串或回调函数。当`ref`属性是回调函数时，该函数接收底层DOM元素或类实例（取决于元素的类型）作为其参数。这使您可以直接访问DOM元素或组件实例。

谨慎使用参考。如果您发现自己经常在应用中使用参考“让事情发生”，请考虑更熟悉自顶向下的数据流。

## Events

使用React元素处理事件有一些语法上的差异：

- React事件处理程序使用camelCase命名，而不是小写。

- 使用JSX，您将传递函数作为事件处理函数，而不是字符串。

## 和解

当组件的道具或状态发生变化时，React通过比较新返回的元素和先前渲染的元素来决定是否需要实际的DOM更新。当它们不相等时，React将更新DOM。这个过程被称为“和解”。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com