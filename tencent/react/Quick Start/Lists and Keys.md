# Lists and Keys

首先，让我们回顾一下如何在 JavaScript 中转换列表。

给定下面的代码，我们使用`**map()**`函数来获取数组`numbers`并将其值加倍。我们将新返回的数组`map()`赋给变量`doubled`并记录下来：

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

此代码记录`[2, 4, 6, 8, 10]`到控制台。

在 React 中，将数组转换为元素列表几乎完全相同。

### 渲染多个组件

您可以构建元素集合，并使用大括号将其包含在 JSX 中`{}`。

下面，我们`numbers`使用 JavaScript `**map()**`函数遍历数组。我们`<li>`为每个项目返回一个元素。最后，我们将得到的元素数组分配给`listItems`：

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

我们将整个`listItems`数组包含在一个`<ul>`元素中，并将其呈现给 DOM：

```javascript
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

此代码显示1到5之间的数字的项目符号列表。

### 基本列表组件

通常你会在组件内渲染列表。

我们可以将前面的例子重构为一个组件，该组件接受一个数组`numbers`并输出一个无序的元素列表。

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

当你运行这段代码时，会给你一个警告，即应该为列表项提供一个密钥。“键”是在创建元素列表时需要包含的特殊字符串属性。我们将在下一节讨论为什么它很重要。

让我们将内容分配`key`给我们的列表项`numbers.map()`并修复缺失的关键问题。

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

## 按键

键可帮助 React 识别哪些项目已更改，添加或删除。键应该赋予数组内的元素以给元素一个稳定的标识：

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

选择密钥的最佳方法是使用一个字符串，它唯一标识其兄弟中的列表项。大多数情况下，您会使用数据中的 ID 作为关键字：

```javascript
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

如果您没有渲染项目的稳定 ID，则可以使用项目索引作为最后的手段：

```javascript
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

如果这些项目可以重新排序，我们不建议使用索引，因为这会很慢。如果您有兴趣，您可以阅读深入的解释，说明为什么密钥是必需的。

### 用键提取组件

键仅在周围阵列的情况下才有意义。

例如，如果你提取一个`ListItem`组件，你应该保留`<ListItem />`数组中元素的关键，而不是它本身的根`<li>`元素`ListItem`。

**示例：不正确的密钥用法**

```javascript
function ListItem(props) {
  const value = props.value;
  return (
    // Wrong! There is no need to specify the key here:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**示例：正确的密钥用法**

```javascript
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

一个好的经验法则是`map()`调用中的元素需要键。

### 钥匙必须在兄弟姐妹中独一无二

在阵列中使用的键在他们的兄弟姐妹中应该是唯一的。但是，他们不需要全球独一无二。当我们生成两个不同的数组时，我们可以使用相同的键：

```javascript
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

Keys 可以作为 React 的提示，但不会传递给组件。如果您的组件中需要相同的值，请将其明确传递为具有不同名称的道具：

```javascript
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

通过上面的例子，`Post`组件可以读取`props.id`，但不能`props.key`。

### 在 JSX 中嵌入 map（）

在上面的例子中，我们声明了一个单独的`listItems`变量并将其包含在 JSX 中：

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX 允许在花括号中嵌入任何表达式，以便我们可以嵌入`map()`结果：

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

**在 CodePen 上试用它。**

有时候会导致代码更加清晰，但这种风格也可能被滥用。就像在 JavaScript 中一样，由您决定是否值得提取变量以提高可读性。请记住，如果`map()`主体太嵌套，则可能是提取组件的好时机。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com