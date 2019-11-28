# Rendering Elements

元素是 React 应用程序的最小构建块。

一个元素描述了你想在屏幕上看到的内容：

```javascript
const element = <h1>Hello, world</h1>;
```

与浏览器 DOM 元素不同，React 元素是普通对象，创建起来很便宜。React DOM 负责更新 DOM 以匹配 React 元素。

> **注意：** 人们可能会将元素与更广为人知的“组件”概念混为一谈。我们将在下一部分介绍组件。元素是什么组成部分“由”组成，我们鼓励您在跳跃前阅读本节。

## 将元素渲染到 DOM 中

假设`<div>`您的 HTML 文件中存在某个地方：

```javascript
<div id="root"></div>
```

我们称之为“根”DOM 节点，因为它内部的所有内容都将由 React DOM 进行管理。

仅使用 React 构建的应用程序通常具有单个根 DOM 节点。如果您将 React 集成到现有的应用程序中，则可以根据需要拥有尽可能多的独立根 DOM 节点。

要将 React 元素渲染到根 DOM 节点中，请将两者传递给`ReactDOM.render()`：

```javascript
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

**在 CodePen 上试用它。**

它在页面上显示“Hello，world”。

## 更新呈现的元素

React 元素是**不可变的**。一旦创建了一个元素，就不能更改其子元素或属性。元素就像电影中的单个框架：它表示某个时间点的用户界面。

根据我们目前的知识，更新 UI 的唯一方法是创建一个新元素并将其传递给`ReactDOM.render()`。

考虑这个滴答作响的时钟示例：

```javascript
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

**在 CodePen 上试用它。**

它每秒从`**setInterval()**`回调中调用`ReactDOM.render()`。

> **注意：** 实际上，大多数 React 应用程序只会调用`ReactDOM.render()`一次。在接下来的部分中，我们将学习如何将这些代码封装到有状态的组件中。我们建议您不要跳过主题，因为它们互相构建。

## 反应只是更新什么是必要的

React DOM 将元素及其子元素与前一元素进行比较，并仅应用必要的 DOM 更新以使 DOM 达到所需的状态。

您可以通过使用浏览器工具检查**最后一个示例**来进行验证：

![img](https://ask.qcloudimg.com/http-save/devdocs/tl8mqpylmb.gif)

即使我们创建了一个元素来描述整个 UI 树，但只有内容已更改的文本节点才会被 React DOM 更新。

根据我们的经验，思考如何在任何特定时刻看到 UI，而不是随着时间的推移如何改变，从而消除了一整类错误。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com