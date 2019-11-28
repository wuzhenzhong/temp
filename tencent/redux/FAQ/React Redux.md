# Redux FAQ: React Redux

- 贡献者1人

  

## 目录

- 为什么不是我的组件重新渲染，或我的 mapStateToProps 运行？



[纠错](javascript:;)

- 为什么我的组件经常重新渲染？

- 我怎样才能加快我的 mapStateToProps？

- 为什么我没有在连接组件中使用 this.props.dispatch？

- 我应该只连接顶层组件，还是可以连接树中的多个组件？

## React Redux

### 为什么不是我的组件重新渲染，或我的 mapStateToProps 运行？

直接意外地改变或修改你的状态是迄今为止，组件在调度动作后不重新渲染的最常见原因。Redux 希望你的减员会“不变”地更新他们的状态，这实际上意味着总是复制你的数据，并将你的修改应用到副本中。如果您从还原器返回相同的对象，Redux 会假定没有任何更改，即使您对其内容进行了更改。同样，`shouldComponentUpdate`中的 React Redux 会尝试通过对传入道具进行浅平等引用检查来提高性能，如果所有引用都相同，则返回`false`跳过实际更新原始组件。

重要的是要记住，无论何时更新嵌套值，还必须在状态树中返回其上的任何新副本。如果你有`state.a.b.c.d`，你想使一个更新`d`，您还需要返回的新副本`c`，`b`，`a`，和`state`。此[状态树突变图](http://arqex.com/wp-content/uploads/2015/02/trees.png)演示了树中的深度变化需要如何改变。

请注意，“更新数据不可改变”不*不*意味着你必须使用[Immutable.js](https://facebook.github.io/immutable-js/)，尽管这肯定是一种选择。你可以使用几种不同的方法对普通的JS对象和数组进行不可变的更新：

- 使用的功能，如复制的对象`Object.assign()`或`_.extend()`，并且阵列的功能，例如`slice()`与`concat()`

- ES6中的阵列扩展运算符，以及为将来版本的JavaScript提出的类似对象扩展运算符

- 将不可变更新逻辑包装成简单函数的实用程序库

#### 更多信息

**文档**

- Troubleshooting（故障排除）

- [React Redux: Troubleshooting（](https://github.com/reactjs/react-redux/blob/master/docs/troubleshooting.md)React Redux：疑难解答[）](https://github.com/reactjs/react-redux/blob/master/docs/troubleshooting.md)

- Recipes: Using the Object Spread Operator（使用对象传播操作符）

- Recipes: Structuring Reducers - Prerequisite Concepts（构建Reducers - 先决条件的概念）

- Recipes: Structuring Reducers - Immutable Update Patterns（构建 Reducers - 不变的更新模式）

**文章**

- [Pros and Cons of Using Immutability with React（](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)反应法使用不变性的优缺点[）](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

- [React/Redux Links: Immutable Data（](https://github.com/markerikson/react-redux-links/blob/master/immutable-data.md)React/Redux链接：不可变数据[）](https://github.com/markerikson/react-redux-links/blob/master/immutable-data.md)

**讨论**

- [#1262: Immutable data + bad performance（](https://github.com/reactjs/redux/issues/1262)不可变的数据+糟糕的表现[）](https://github.com/reactjs/redux/issues/1262)

- [React Redux #235: Predicate function for updating component（](https://github.com/reactjs/react-redux/issues/235)用于更新组件的谓词函数[）](https://github.com/reactjs/react-redux/issues/235)

- [React Redux #291: Should mapStateToProps be called every time an action is dispatched?（](https://github.com/reactjs/react-redux/issues/291)每次调度操作时，是否应调用 mapStateToProps？[）](https://github.com/reactjs/react-redux/issues/291)

- [Stack Overflow: Cleaner/shorter way to update nested state in Redux?（](http://stackoverflow.com/questions/35592078/cleaner-shorter-way-to-update-nested-state-in-redux)堆栈溢出：更新/更短的方式来更新 Redux 中的嵌套状态？[）](http://stackoverflow.com/questions/35592078/cleaner-shorter-way-to-update-nested-state-in-redux)

- [Gist: state mutations（](https://gist.github.com/amcdnl/7d93c0c67a9a44fe5761#gistcomment-1706579)要点：状态突变[）](https://gist.github.com/amcdnl/7d93c0c67a9a44fe5761#gistcomment-1706579)

### 为什么我的组件经常重新渲染？

React Redux 实现了几个优化，以确保实际组件只在实际需要时重新渲染。其中之一是对由传递给`mapStateToProps`和的`mapDispatchToProps`参数生成的组合道具对象进行浅层等式检查`connect`。不幸的是，在每次`mapStateToProps`调用新创建的数组或对象实例的情况下，浅的平等不起作用。一个典型的例子可能是映射一个ID数组并返回匹配的对象引用，例如：

```javascript
const mapStateToProps = state => {
  return {
    objects: state.objectIds.map(id => state.objects[id])
  }
}
```

即使数组可能每次都包含完全相同的对象引用，但数组本身是不同的引用，因此浅平等检查失败，并且 React Redux 将重新呈现包装组件。

额外的重新渲染可以通过使用 reducer 将对象数组保存到状态，使用 [Reselect ](https://github.com/reactjs/reselect)缓存映射的数组，或者`shouldComponentUpdate`手动实现组件并使用一个函数（例如）执行更深入的道具比较来解决`_.isEqual`。注意不要让自定义`shouldComponentUpdate()`比渲染本身更昂贵！始终使用分析器来检查您对性能的假设。

对于未连接的组件，您可能需要检查传入的道具是什么。常见问题是父组件在其渲染函数内重新绑定回调，例如`<Child` `onClick={this.handleClick.bind(this)}` `/>`。每次父级重新呈现时，都会创建一个新的函数引用。在父组件的构造函数中只绑定一次回调通常是很好的做法。

#### 更多信息

**文档**

- FAQ: Performance - Scaling（性能缩放）

### **文章**

- [A Deep Dive into React Perf Debugging（](http://benchling.engineering/deep-dive-react-perf-debugging/)深入研究React Perf调试[）](http://benchling.engineering/deep-dive-react-perf-debugging/)

- [React.js pure render performance anti-pattern（](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f)React.js纯粹渲染性能反模式[）](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f)

- [Improving React and Redux Performance with Reselect（](http://blog.rangle.io/react-and-redux-performance-with-reselect/)用重新选择提高 React 和 Redux 的性能[）](http://blog.rangle.io/react-and-redux-performance-with-reselect/)

- [Encapsulating the Redux State Tree（](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)封装 Redux 状态树[）](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)

- [React/Redux Links: React/Redux Performance（](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md)React/Redux链接：React/Redux性能[）](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md)

**讨论**

- [Stack Overflow: Can a React Redux app scale as well as Backbone?（](http://stackoverflow.com/questions/34782249/can-a-react-redux-app-really-scale-as-well-as-say-backbone-even-with-reselect)堆栈溢出：React Redux 应用程序可以像 Backbone 一样扩展吗？[）](http://stackoverflow.com/questions/34782249/can-a-react-redux-app-really-scale-as-well-as-say-backbone-even-with-reselect)

### **Libraries**

- [Redux Addons Catalog: DevTools - Component Update Monitoring](https://github.com/markerikson/redux-ecosystem-links/blob/master/devtools.md#component-update-monitoring)

### 我如何加快我的速度`mapStateToProps`？

尽管 React Redux 可以最大限度地减少`mapStateToProps`调用函数的次数，但确保`mapStateToProps`快速运行并尽量减少工作量仍然是一个不错的主意。常用的推荐方法是使用 [Reselect ](https://github.com/reactjs/reselect)创建 memoized“选择器”功能。这些选择器可以组合在一起，稍后在管道中的选择器只有在其输入发生变化时才会运行。这意味着您可以创建选择器来执行筛选或排序等任务，并确保只有在需要时才会发生实际工作。

#### 更多信息

**文档**

- Recipes: Computed Derived Data（计算派生数据）

### **文章**

- [Improving React and Redux Performance with Reselect（](http://blog.rangle.io/react-and-redux-performance-with-reselect/)用重新选择提高 React 和 Redux 的性能[）](http://blog.rangle.io/react-and-redux-performance-with-reselect/)

**讨论**

- [#815: Working with Data Structures（](https://github.com/reactjs/redux/issues/815)使用数据结构[）](https://github.com/reactjs/redux/issues/815)

- [Reselect #47: Memoizing Hierarchical Selectors（](https://github.com/reactjs/reselect/issues/47)记忆分层选择器[）](https://github.com/reactjs/reselect/issues/47)

### 为什么我不能在我的连接组件中使用`this.props.dispatch`**?**



`connect()`函数有两个主要参数，都是可选的。第一个，`mapStateToProps`是你提供的一个函数，当它改变时从商店中提取数据，并将这些值作为道具传递给你的组件。第二个，`mapDispatchToProps`是你提供的一个函数，可以使用商店的`dispatch`功能，通常通过创建动作创建者的预先绑定版本，一旦被调用就会自动发送他们的动作。

如果您的`mapDispatchToProps`在调用时没有提供自己的功能`connect()`，React Redux 将提供一个默认版本，它只是将该`dispatch`函数作为道具返回。这意味着，如果你*做的*提供自己的功能，`dispatch`是*不会*自动提供。如果你仍然希望它可以作为道具，你需要在你的`mapDispatchToProps`实现中自己明确地返回它。

#### 更多信息

**文档**

- [React Redux API: connect()](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)

### **讨论**

- [React Redux #89: can i wrap multi actionCreators into one props with name?（](https://github.com/reactjs/react-redux/issues/89)React Redux＃89：我可以将多个 actionCreators 包装成一个名称为 props 的道具吗？[）](https://github.com/reactjs/react-redux/issues/89)

- [React Redux #145: consider always passing down dispatch regardless of what mapDispatchToProps does（](https://github.com/reactjs/react-redux/issues/145)React Redux＃145：不管 mapDispatchToProps 是做什么的，都要考虑总是向下传递[）](https://github.com/reactjs/react-redux/issues/145)

- [React Redux #255: this.props.dispatch is undefined if using mapDispatchToProps（](https://github.com/reactjs/react-redux/issues/255)React Redux＃255：如果使用 mapDispatchToProps，则 this.props.dispatch 未定义[）](https://github.com/reactjs/react-redux/issues/255)

- Stack Overflow: How to get simple dispatch from this.props using connect w/ Redux?（堆栈溢出：如何使用连接w/Redux从this.props获得简单分发？）

### 我应该只连接顶层组件，还是可以连接树中的多个组件？

早期的 Redux 文档建议您应该只在组件树顶部附近有几个连接的组件。然而，时间和经验表明，通常需要少数组件来了解所有后代的数据需求，并迫使它们传递混乱的道具数量。

当前建议的最佳做法是将组件分类为“呈现”或“容器”组件，并在任何有意义的地方提取连接的容器组件：

> Redux 示例中强调“顶部的一个容器组件”是一个错误。不要把这当成一句格言。尽量让您的演示文稿组件分开。在方便时通过连接来创建容器组件。每当你觉得你正在复制父组件中的代码来为相同种类的孩子提供数据时，就需要花费时间来提取一个容器。一般情况下，只要您觉得父级对子的“个人”数据或行为知之甚多，就有时间提取容器。

事实上，基准测试表明，连接的组件越多，连接组件的数量越少，性能越好。

一般来说，尝试在可理解的数据流和组件的责任区域之间找到平衡点。

#### 更多信息

**文档**

- Basics: Usage with React（基础：使用React）

- FAQ: Performance - Scaling（性能—缩放）

**文章**

- [Presentational and Container Components（](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)展示和容器组件[）](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

- [High-Performance Redux（](http://somebody32.github.io/high-performance-redux/)高性能Redux[）](http://somebody32.github.io/high-performance-redux/)

- [React/Redux Links: Architecture - Redux Architecture（](https://github.com/markerikson/react-redux-links/blob/master/react-redux-architecture.md#redux-architecture)React/Redux链接：体系结构 - Redux体系结构[）](https://github.com/markerikson/react-redux-links/blob/master/react-redux-architecture.md#redux-architecture)

- [React/Redux Links: Performance - Redux Performance（](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md#redux-performance)React/Redux 链接：性能 - Redux 性能[）](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md#redux-performance)

**讨论**

- [Twitter: emphasizing “one container” was a mistake（](https://twitter.com/dan_abramov/status/668585589609005056)Twitter：强调“一个容器”是一个错误[）](https://twitter.com/dan_abramov/status/668585589609005056)

- [#419: Recommended usage of connect（](https://github.com/reactjs/redux/issues/419)建议使用连接[）](https://github.com/reactjs/redux/issues/419)

- [#756: container vs component?（](https://github.com/reactjs/redux/issues/756)容器vs组件？[）](https://github.com/reactjs/redux/issues/756)

- [#1176: Redux+React with only stateless components（](https://github.com/reactjs/redux/issues/1176)Redux +仅与无状态组件进行反应[）](https://github.com/reactjs/redux/issues/1176)

- [Stack Overflow: can a dumb component use a Redux container?（](http://stackoverflow.com/questions/34992247/can-a-dumb-component-use-render-redux-container-component)堆栈溢出：愚蠢的组件可以使用 Redux 容器吗？[）](http://stackoverflow.com/questions/34992247/can-a-dumb-component-use-render-redux-container-component)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18