# Introducing JSX

考虑这个变量声明：

```javascript
const element = <h1>Hello, world!</h1>;
```

这个有趣的标签语法既不是字符串也不是HTML。

它被称为JSX，它是JavaScript的语法扩展。我们建议在React中使用它来描述UI的外观。JSX可能会提醒您一种模板语言，但它具有JavaScript的全部功能。

JSX生成React“元素”。我们将在下一节探讨将它们呈现给DOM。在下面，您可以找到启动JSX所需的基础知识。

### 在JSX中嵌入表达式

您可以通过将其包含在大括号中来嵌入JSX中的任何[JavaScript表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。

例如`2 + 2`，`user.firstName`和`formatName(user)`都是有效的表达式：

```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在CodePen上试用它](https://reactjs.org/redirect-to-codepen/introducing-jsx)。

为了便于阅读，我们将JSX分成多行。尽管这不是必需的，但当我们这样做时，我们还建议将它包装在括号中以避免[自动分号插入](http://stackoverflow.com/q/2846283)的缺陷。

### JSX也是一个表达式

编译之后，JSX表达式就变成了常规的JavaScript对象。

这意味着您可以在`if`语句和`for`循环中使用JSX ，将其分配给变量，将其作为参数接受，并从函数中返回：

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### 使用JSX指定属性

您可以使用引号将字符串文字指定为属性：

```javascript
const element = <div tabIndex="0"></div>;
```

您也可以使用大括号将JavaScript表达式嵌入属性中：

```javascript
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入JavaScript表达式时，请勿将大括号括起来。您应该使用引号（用于字符串值）或大括号（用于表达式），但不能同时使用同一个属性。

> **警告：** 由于JSX比HTML更接近JavaScript，因此React DOM使用`camelCase`属性命名约定而不是HTML属性名称。例如，`class`变成[`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)JSX，`tabindex`成为[`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。

### 用JSX指定子

如果标签为空，您可以立即关闭它`/>`，例如XML：

```javascript
const element = <img src={user.avatarUrl} />;
```

JSX标签可能包含子项：

```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX防止注入攻击

在JSX中嵌入用户输入是安全的：

```javascript
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

默认情况下，React DOM 在渲染之前[转义](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)嵌入在JSX中的任何值。因此它确保您永远不会注入任何未明确写入应用程序的内容。在呈现之前，所有内容都会转换为字符串。这有助于防止[XSS（跨站点脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

### JSX表示对象

Babel将JSX编译为`React.createElement()`调用。

这两个例子是相同的：

```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` 执行一些检查以帮助您编写无错代码，但本质上它会创建一个如下所示的对象：

```javascript
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

这些对象被称为“React元素”。你可以把它们想象成你想要在屏幕上看到的东西的描述。React读取这些对象并使用它们来构建DOM并使其保持最新状态。

我们将在下一节探讨将React元素渲染到DOM。

> **提示：** 我们建议您使用[“Babel”语言定义](http://babeljs.io/docs/editors)用于所选编辑器，以便ES6和JSX代码都能正确突出显示。本网站使用与其兼容的[Oceanic Next](https://labs.voronianski.com/oceanic-next-color-scheme/)色彩方案。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com