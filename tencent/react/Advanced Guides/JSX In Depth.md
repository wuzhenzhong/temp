# JSX In Depth

基本上，JSX只是为`React.createElement(component, props, ...children)`函数提供语法糖。JSX代码：

```javascript
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

编译成：

```javascript
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

如果没有子，您也可以使用标签的自闭形式。所以：

```javascript
<div className="sidebar" />
```

编译成：

```javascript
React.createElement(
  'div',
  {className: 'sidebar'},
  null
)
```

如果你想测试一些特定的JSX如何转换成JavaScript，你可以尝试[在线的Babel编译器](https://babeljs.io/repl/#?presets=react&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyA)。

## 指定React元素类型

JSX标签的第一部分决定了React元素的类型。

大写字母表示JSX标记引用了React组件。这些标记会被编译为对指定变量的直接引用，因此如果使用JSX `<Foo />`表达式，则`Foo`必须在范围内。

### 反应必须在范围内

由于JSX编译为调用`React.createElement`，`React`库必须始终处于JSX代码的范围内。

例如，这个代码中的两个导入都是必需的，尽管`React`并`CustomButton`没有直接引用JavaScript：

```javascript
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```

如果您不使用JavaScript捆绑程序并从`<script>`代码加载React ，则它已经在`React`全局范围内。

### 为JSX类型使用点符号

您也可以在JSX中使用点符号来引用React组件。如果您有一个导出许多React组件的单个模块，这很方便。例如，如果`MyComponents.DatePicker`是组件，则可以直接从JSX使用它：

```javascript
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

### 用户定义的组件必须被大写

当元素类型以小写字母开头时，它指的是像`<div>`or 这样的内置组件，`<span>`并生成一个字符串`'div'`或`'span'`传递给它`React.createElement`。以大写字母开头的类型，例如`<Foo />`编译`React.createElement(Foo)`并对应于JavaScript文件中定义或导入的组件。

我们建议用大写字母命名组件。如果您确实有一个以小写字母开头的组件，请在将它用于JSX之前将其分配给大写变量。

例如，这段代码不会按预期运行：

```javascript
import React from 'react';

// Wrong! This is a component and should have been capitalized:
function hello(props) {
  // Correct! This use of <div> is legitimate because div is a valid HTML tag:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // Wrong! React thinks <hello /> is an HTML tag because it's not capitalized:
  return <hello toWhat="World" />;
}
```

为了解决这个问题，我们将重新命名`hello`，以`Hello`和使用`<Hello />`提到它的时候：

```javascript
import React from 'react';

// Correct! This is a component and should be capitalized:
function Hello(props) {
  // Correct! This use of <div> is legitimate because div is a valid HTML tag:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // Correct! React knows <Hello /> is a component because it's capitalized.
  return <Hello toWhat="World" />;
}
```

### 在运行时选择类型

您不能使用通用表达式作为React元素类型。如果您确实想使用通用表达式来指示元素的类型，请将其首先分配给大写变量。当你想渲染一个基于道具的不同组件时，通常会出现这种情况：

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
}
```

为了解决这个问题，我们首先将类型赋值给一个大写的变量：

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

## JSX中的道具

有几种不同的方式可以在JSX中指定道具。

### JavaScript表达式作为道具

您可以将任何JavaScript表达式作为道具传递，通过将其包围`{}`。例如，在这个JSX中：

```javascript
<MyComponent foo={1 + 2 + 3 + 4} />
```

因为`MyComponent`，值的`props.foo`将是`10`因为表达式`1 + 2 + 3 + 4`被评估。

`if`语句和`for`循环在JavaScript中不是表达式，所以它们不能直接在JSX中使用。相反，你可以把它们放在周围的代码中。例如：

```javascript
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

您可以在相应章节中了解有关条件渲染和循环的更多信息。

### 字符串文字

你可以传递一个字符串作为道具。这两个JSX表达式是等价的：

```javascript
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

当你传递一个字符串时，它的值是HTML-unescaped。所以这两个JSX表达式是等价的：

```javascript
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

这种行为通常不相关。这里只提到完整性。

### 道具默认为“真”

如果您没有为道具传递值，则默认为`true`。这两个JSX表达式是等价的：

```javascript
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

一般来说，我们不建议使用它，因为它可能与[ES6对象速记](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015) 相混淆，而[速记](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015)`{foo}`是简短的，`{foo: foo}`而不是`{foo: true}`。这种行为就在那里，以便它符合HTML的行为。

### 传播属性

如果你已经有了`props`一个对象，并且你想通过JSX传递它，你可以使用`...`“spread”运算符来传递整个道具对象。这两个组件是等价的：

```javascript
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

你也可以选择你的组件将使用的特定道具，同时使用传播运算符传递所有其他道具。

```javascript
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};

const App = () => {
  return (
    <div>
      <Button kind="primary" onClick={() => console.log("clicked!")}>
        Hello World!
      </Button>
    </div>
  );
};
```

在上面的例子中，`kind`道具被安全地消费，*并且不会*传递给`<button>`DOM中的元素。所有其他道具都通过`...other`对象传递，使得这个组件非常灵活。你可以看到，它传递一个`onClick`和`children`道具。

扩展属性可能很有用，但它们还可以轻松地将不必要的道具传递给不关心它们的组件或将无效的HTML属性传递给DOM。我们建议谨慎使用这种语法。

## JSX的儿童

在同时包含开始标记和结束标记的JSX表达式中，这些标记之间的内容作为特殊的道具传递：`props.children`。有几种不同的方式来传递孩子：

### 字符串文字

您可以在开始标签和结束标签之间放置一个字符串，并且`props.children`只是该字符串。这对于许多内置的HTML元素很有用。例如：

```javascript
<MyComponent>Hello world!</MyComponent>
```

这是有效的JSX，并`props.children`在`MyComponent`将简单地字符串`"Hello world!"`。HTML是非转义的，所以通常可以编写JSX，就像您以这种方式编写HTML一样：

```javascript
<div>This is valid HTML &amp; JSX at the same time.</div>
```

JSX在行的开头和结尾删除空格。它也删除空行。与标签相邻的新行被删除; 发生在字符串文字中间的新行会压缩为一个空格。所以这些都呈现相同的事物：

```javascript
<div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

### JSX儿童

您可以提供更多的JSX元素作为孩子。这对于显示嵌套组件很有用：

```javascript
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```

您可以将不同类型的子项混合在一起，以便您可以将字符串文字与JSX子项一起使用。这是JSX与HTML相似的另一种方式，因此这是有效的JSX和有效的HTML：

```javascript
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>
```

React组件也可以返回一组元素：

```javascript
render() {
  // No need to wrap list items in an extra element!
  return [
    // Don't forget the keys :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```

### 作为儿童的JavaScript表达式

您可以将任何JavaScript表达式作为子项传递，方法是将其放入其中`{}`。例如，这些表达式是等价的：

```javascript
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

这通常用于呈现任意长度的JSX表达式列表。例如，这呈现一个HTML列表：

```javascript
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```

JavaScript表达式可以与其他类型的孩子混合使用。这通常用于替代字符串模板：

```javascript
function Hello(props) {
  return <div>Hello {props.addressee}!</div>;
}
```

### 作为儿童的函数

通常，插入到JSX中的JavaScript表达式将评估为一个字符串，一个React元素或这些东西的列表。然而，`props.children`就像任何其他道具一样，它可以传递任何类型的数据，而不仅仅是React知道如何渲染的种类。例如，如果你有一个自定义组件，你可以让它回调为`props.children`：

```javascript
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

传递给自定义组件的子项可以是任何东西，只要该组件将它们转换为React在呈现之前可以理解的内容即可。这种用法并不常见，但如果您想要扩展JSX的功能，它就可以工作。

### 布尔值，空值和未定义被忽略

`false`，`null`，`undefined`，和`true`有效的儿童。他们根本不渲染。这些JSX表达式都将呈现相同的事物：

```javascript
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

这有助于有条件地呈现React元素。这JSX只呈现一个`<Header />`，如果`showHeader`是`true`：

```javascript
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

有一点需要注意的是，一些[“虚假”的价值观](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)，如`0`数字，仍然由React渲染。例如，这段代码不会像你所期望的那样工作，因为`0`当它`props.messages`是一个空数组的时候会被打印出来：

```javascript
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
```

要解决这个问题，请确保前面的表达式`&&`始终是布尔值：

```javascript
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```

相反，如果你想有一个类似的值`false`，`true`，`null`，或`undefined`出现在输出中，你必须[把它转换为字符串](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#String_conversion)第一：

```javascript
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18