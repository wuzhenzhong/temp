# Components and Props

组件让你可以将用户界面分成独立的，可重复使用的部分，并且可以独立思考每一部分。

从概念上讲，组件就像JavaScript函数一样。他们接受任意输入（称为“道具”）并返回描述屏幕上应显示内容的React元素。

## 功能和类组件

定义组件的最简单方法是编写一个JavaScript函数：

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

这个函数是一个有效的React组件，因为它接受一个包含数据的“props”（代表属性）对象参数并返回一个React元素。我们称这些组件为“功能性”，因为它们实际上是JavaScript功能。

您也可以使用[ES6类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)来定义组件：

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

以上两个组件与React的观点相同。

类有一些额外的功能，我们将在下面的章节中讨论。在此之前，我们将使用功能组件来简洁。

## 渲染组件

以前，我们只遇到了表示DOM标签的React元素：

```javascript
const element = <div />;
```

但是，元素也可以表示用户定义的组件：

```javascript
const element = <Welcome name="Sara" />;
```

当React查看表示用户定义组件的元素时，它会将JSX属性作为单个对象传递给此组件。我们称这个对象为“道具”。

例如，这段代码在页面上呈现“Hello，Sara”：

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在CodePen上试用它](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component)。

让我们回顾一下在这个例子中发生了什么：

1. 我们呼吁`ReactDOM.render()`与`<Welcome name="Sara" />`元素。

\2. React调用`Welcome`组件`{name: 'Sara'}`作为道具。

\3. 我们的`Welcome`组件返回一个`<h1>Hello, Sara</h1>`元素作为结果。

\4. React DOM有效地更新DOM以匹配`<h1>Hello, Sara</h1>`。

> **警告：** 始终用大写字母开始组件名称。例如，`<div />`表示一个DOM标签，但`<Welcome />`表示一个组件，并且需要`Welcome`在范围内。

## 构成组件

组件可以在其输出中引用其他组件。这让我们可以对任何细节级别使用相同的组件抽象。一个按钮，一个窗体，一个对话框，一个屏幕：在React应用程序中，所有这些都通常表示为组件。

例如，我们可以创建一个`App`呈现`Welcome`多次的组件：

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[在CodePen上试用](https://reactjs.org/redirect-to-codepen/components-and-props/composing-components)。

通常情况下，新的React应用程序`App`在顶部有一个组件。但是，如果您将React集成到现有的应用程序中，则可能会自下而上地使用一个小组件，`Button`并逐渐走向视图层次结构的顶部。

## 提取组件

不要害怕将组件分解成更小的组件。

例如，考虑这个`Comment`组件：

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[在CodePen上试用](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components)。

它接受`author`（一个对象），`text`（一个字符串）和`date`（一个日期）作为道具，并描述了对社交媒体网站的评论。

由于所有的嵌套，这个组件可能会变得很棘手，并且很难重用它的各个部分。我们从中提取一些组件。

首先，我们将提取`Avatar`：

```javascript
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

在`Avatar`不需要知道它正在呈现内`Comment`。这就是为什么我们给它的道具一个更通用的名称：`user`而不是`author`。

我们建议从组件自己的角度命名道具，而不是使用它的环境。

我们现在可以简化`Comment`一点点：

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

接下来，我们将提取一个`UserInfo`呈现`Avatar`用户名旁边的组件：

```javascript
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

这可以让我们`Comment`进一步简化：

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[在CodePen上试用](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components-continued)。

首先提取组件可能看起来像是咕噜咕噜的工作，但在更大的应用程序中有可重用组件的调色板。一个好的经验法则是，如果你的用户界面的一部分被多次使用（`Button`，`Panel`，`Avatar`），或者是对自己足够复杂（`App`，`FeedStory`，`Comment`），这是一个很好的候选人是一个可重用的组件。

## 道具是只读的

无论你是将一个组件声明为一个函数还是一个类，它都不能修改它自己的道具。考虑这个`sum`功能：

```javascript
function sum(a, b) {
  return a + b;
}
```

这些函数被称为[“纯粹的”，](https://en.wikipedia.org/wiki/Pure_function)因为它们不会尝试改变它们的输入，并且总是为相同的输入返回相同的结果。

相反，这个函数是不纯的，因为它改变了它自己的输入：

```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```

React非常灵活，但它有一个严格的规则：

**所有的React组件都必须像纯粹的道具一样行事。**

当然，应用程序用户界面是动态的，并随着时间而改变。在下一节中，我们将介绍一个新的“国家”概念。状态允许React组件在不违反此规则的情况下响应用户操作，网络响应和其他任何内容而随时间改变其输出。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18