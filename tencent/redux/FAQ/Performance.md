# Redux FAQ：性能（Performance）

- 贡献者2人

  

## 目录

- Redux 在性能和体系结构方面的“规模”如何？

- 不会为每个动作调用“所有减速器”会很慢吗？

- 我是否需要在减速器中克隆我的状态？是不是复制我的状态会变慢？

- 我如何减少商店更新事件的数量？

- “拥有一棵树”会导致内存问题吗？将调度许多行动占用内存？

## 性能

### Redux 在性能和体系结构方面的“规模”如何？

尽管对此没有单一的明确答案，但大多数情况下，这两者都不应成为问题。

Redux 所做的工作通常包括以下几个方面：在中间件和简化器中处理操作（包括用于不可变更新的对象复制），在调度操作之后通知订户，以及根据状态更改更新UI组件。尽管在充分复杂的情况下，每种方法都*可能*成为性能问题，但 Redux 的实施方式并没有什么本质上的缓慢或低效。事实上，React Redux 尤其经过了大量优化，可以减少不必要的重新渲染，React-Redux v5相对于早期版本显示出明显的改进。

与其他库相比，Redux 可能没有那么高效。为了在 React 应用程序中获得最大的渲染性能，应该将状态存储为规范化的形状，许多单独的组件应该连接到商店而不是几个，并且连接列表组件应该将项目ID传递到其连接的子项列表项（允许列出项目以通过 ID 查找他们自己的数据）。这最大限度地减少了要完成的渲染总量。记忆选择器功能的使用也是重要的性能考虑因素。

至于架构，轶事证据表明，Redux 适用于不同的项目和团队规模。Redux 目前由数百家公司和数千名开发人员使用，每月有数十万次来自 NPM 的安装。一名开发人员报告

> 对于规模，我们有〜500个动作类型，〜400个减速器案例，〜150个组件，5个中间件，〜200个动作，〜2300个测试

#### 更多信息

**文档**

- Recipes: Structuring Reducers - Normalizing State Shape（构造减速器 - 规范化状态形状）

### **文档**

- [How to Scale React Applications](https://www.smashingmagazine.com/2016/09/how-to-scale-react-applications/) (accompanying talk: [Scaling React Applications](https://vimeo.com/168648012))（如何扩展React应用程序（随附讨论：缩放React应用程序））

- [High-Performance Redux（](http://somebody32.github.io/high-performance-redux/)高性能Redux[）](http://somebody32.github.io/high-performance-redux/)

- [Improving React and Redux Perf with Reselect（](http://blog.rangle.io/react-and-redux-performance-with-reselect/)重新选择改进React和Redux Perf[）](http://blog.rangle.io/react-and-redux-performance-with-reselect/)

- [Encapsulating the Redux State Tree（](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)封装Redux状态树[）](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)

- [React/Redux Links: Performance - Redux（](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md#redux-performance)React/Redux链接：性能 - Redux[）](https://github.com/markerikson/react-redux-links/blob/master/react-performance.md#redux-performance)

**讨论**

- [#310: Who uses Redux?（](https://github.com/reactjs/redux/issues/310)谁使用Redux？[）](https://github.com/reactjs/redux/issues/310)

- [#1751: Performance issues with large collections（](https://github.com/reactjs/redux/issues/1751)大集合的性能问题[）](https://github.com/reactjs/redux/issues/1751)

- [React Redux #269: Connect could be used with a custom subscribe method（](https://github.com/reactjs/react-redux/issues/269)Connect可以与自定义订阅方法一起使用[）](https://github.com/reactjs/react-redux/issues/269)

- [React Redux #407: Rewrite connect to offer an advanced API（](https://github.com/reactjs/react-redux/issues/407)重写连接以提供高级API[）](https://github.com/reactjs/react-redux/issues/407)

- [React Redux #416: Rewrite connect for better performance and extensibility（](https://github.com/reactjs/react-redux/pull/416)重写连接以获得更好的性能和可扩展性[）](https://github.com/reactjs/react-redux/pull/416)

- [Redux vs MobX TodoMVC Benchmark: #1（](https://github.com/mweststrate/redux-todomvc/pull/1)Redux与MobX TodoMVC Benchmark:＃1[）](https://github.com/mweststrate/redux-todomvc/pull/1)

- [Reddit: What's the best place to keep the initial state?（](https://www.reddit.com/r/reactjs/comments/47m9h5/whats_the_best_place_to_keep_the_initial_state/)什么是保持初始状态的最佳地点？[）](https://www.reddit.com/r/reactjs/comments/47m9h5/whats_the_best_place_to_keep_the_initial_state/)

- [Reddit: Help designing Redux state for a single page app（](https://www.reddit.com/r/reactjs/comments/48k852/help_designing_redux_state_for_a_single_page/)帮助设计单个页面应用程序的Redux状态[）](https://www.reddit.com/r/reactjs/comments/48k852/help_designing_redux_state_for_a_single_page/)

- [Reddit: Redux performance issues with a large state object?（](https://www.reddit.com/r/reactjs/comments/41wdqn/redux_performance_issues_with_a_large_state_object/)大型状态对象的Redux性能问题？[）](https://www.reddit.com/r/reactjs/comments/41wdqn/redux_performance_issues_with_a_large_state_object/)

- [Reddit: React/Redux for Ultra Large Scale apps（](https://www.reddit.com/r/javascript/comments/49box8/reactredux_for_ultra_large_scale_apps/)React/Redux适用于超大型应用程序[）](https://www.reddit.com/r/javascript/comments/49box8/reactredux_for_ultra_large_scale_apps/)

- [Twitter: Redux scaling（](https://twitter.com/NickPresta/status/684058236828266496)Twitter：Redux缩放[）](https://twitter.com/NickPresta/status/684058236828266496)

- [Twitter: Redux vs MobX benchmark graph - Redux state shape matters（](https://twitter.com/dan_abramov/status/720219615041859584)Twitter：Redux vs MobX 基准图 - Redux 状态形状很重要[）](https://twitter.com/dan_abramov/status/720219615041859584)

- [Stack Overflow: How to optimize small updates to props of nested components?（](http://stackoverflow.com/questions/37264415/how-to-optimize-small-updates-to-props-of-nested-component-in-react-redux)堆栈溢出：如何优化嵌套组件的道具的小更新？[）](http://stackoverflow.com/questions/37264415/how-to-optimize-small-updates-to-props-of-nested-component-in-react-redux)

- [Chat log: React/Redux perf - updating a 10K-item Todo list（](https://gist.github.com/markerikson/53735e4eb151bc228d6685eab00f5f85)聊天记录：React/Redux perf - 更新10K条目待办事项列表[）](https://gist.github.com/markerikson/53735e4eb151bc228d6685eab00f5f85)

- [Chat log: React/Redux perf - single connection vs many connections（](https://gist.github.com/markerikson/6056565dd65d1232784bf42b65f8b2ad)聊天记录：React/Redux perf - 单连接与多个连接[）](https://gist.github.com/markerikson/6056565dd65d1232784bf42b65f8b2ad)

### 不会为每个动作调用“所有减速器”会很慢吗？

重要的是要注意 Redux 商店真的只有一个 Reducer 功能。商店将当前状态和已分派的操作传递给该减速器功能，并让减速器适当地处理事情。

显然，试图处理单个函数中的每一个可能的动作并不能很好地扩展，仅仅是在函数大小和可读性方面，所以将实际工作分割成可以由顶级减速器调用的单独函数是有意义的。特别是，常见的建议模式是具有一个单独的子简化器函数，负责管理特定密钥的特定状态片更新。该`combineReducers()`随终极版是众多可能的方式来实现这一目标之一。同时强烈建议您保持商店状态尽可能平坦和规范。但最终，您负责以任何您想要的方式组织您的减速器逻辑。

但是，即使您碰巧有许多不同的减速机功能组合在一起，并且即使在深度嵌套状态下，减速机速度也不会成为问题。JavaScript 引擎每秒能够运行大量的函数调用，并且大多数 reducers 可能只是使用`switch`语句并默认返回现有状态以响应大多数操作。

如果您确实担心 Reducer 性能，可以使用诸如 [redux-ignore ](https://github.com/omnidan/redux-ignore)或 [reduxr-scoped-reducer 之](https://github.com/chrisdavies/reduxr-scoped-reducer)类的实用程序来确保只有特定的 reducer 监听特定的操作。您也可以使用 [redux-log-slow-redurs ](https://github.com/michaelcontento/redux-log-slow-reducers)来执行一些性能基准测试。

#### 更多信息

**讨论**

- [#912: Proposal: action filter utility（](https://github.com/reactjs/redux/issues/912)建议：操作过滤器实用程序[）](https://github.com/reactjs/redux/issues/912)

- [#1303: Redux Performance with Large Store and frequent updates（](https://github.com/reactjs/redux/issues/1303)大型商店和经常更新的Redux性能[）](https://github.com/reactjs/redux/issues/1303)

- [Stack Overflow: State in Redux app has the name of the reducer（](http://stackoverflow.com/questions/35667775/state-in-redux-react-app-has-a-property-with-the-name-of-the-reducer/35674297)堆栈溢出：Redux 应用程序中的状态具有 reducer 的名称[）](http://stackoverflow.com/questions/35667775/state-in-redux-react-app-has-a-property-with-the-name-of-the-reducer/35674297)

- [Stack Overflow: How does Redux deal with deeply nested models?（](http://stackoverflow.com/questions/34494866/how-does-redux-deals-with-deeply-nested-models/34495397)堆栈溢出：Redux 如何处理深度嵌套模型？[）](http://stackoverflow.com/questions/34494866/how-does-redux-deals-with-deeply-nested-models/34495397)

### 我是否需要在减速器中克隆我的状态？是不是复制我的状态会变慢？

不断更新状态通常意味着制作浅拷贝，而不是深拷贝。浅拷贝比深拷贝要快得多，因为需要拷贝更少的对象和字段，并且有效地归结为移动一些指针。

但是，您*确实*需要为受影响的每个嵌套级别创建一个复制和更新的对象。虽然这不应该特别昂贵，但如果可能的话，为什么你应该保持你的状态正常化和浅层化是另一个很好的理由。

> 常见的 Redux 误解：您需要深入克隆状态。现实：如果里面的东西没有改变，请保持它的参考相同！

#### 更多信息

**文档**

- Recipes: Structuring Reducers - Prerequisite Concepts（构建Reducers - 先决条件的概念）

- Recipes: Structuring Reducers - Immutable Update Patterns（构建Reducers - 不变的更新模式）

**讨论**

- [#454: Handling big states in reducer（](https://github.com/reactjs/redux/issues/454)处理减速器中的大状态[）](https://github.com/reactjs/redux/issues/454)

- [#758: Why can't state be mutated?（](https://github.com/reactjs/redux/issues/758)为什么不能状态变异？[）](https://github.com/reactjs/redux/issues/758)

- [#994: How to cut the boilerplate when updating nested entities?（](https://github.com/reactjs/redux/issues/994)如何在更新嵌套实体时剪切样板文件？[）](https://github.com/reactjs/redux/issues/994)

- [Twitter: common misconception - deep cloning（](https://twitter.com/dan_abramov/status/688087202312491008)Twitter：常见的误解 - 深层克隆[）](https://twitter.com/dan_abramov/status/688087202312491008)

- [Cloning Objects in JavaScript（](http://www.zsoltnagy.eu/cloning-objects-in-javascript/)在 JavaScript 中克隆对象[）](http://www.zsoltnagy.eu/cloning-objects-in-javascript/)

### 我如何减少商店更新事件的数量？

Redux 在每次成功发送操作后通知订阅者（例如，行为到达商店并由减少者处理）。在某些情况下，减少订阅者被调用的次数可能是有用的，特别是如果动作创建者在一行中分派多个不同的动作。

如果您使用 React，请注意您可以通过将它们包装进来来提高多个同步调度的性能`ReactDOM.unstable_batchedUpdates()`，但此 API 是试验性的，可能会在任何 React 版本中删除，因此不要过度依赖。看一看 [redux-batched-actions](https://github.com/tshelburne/redux-batched-actions)（一个更高阶的 reducer，它可以让你分派几个动作，就好像它是一个动作一样，并在 reducer 中“解压缩”），[redux-batched-subscribe](https://github.com/tappleby/redux-batched-subscribe)（一个商店增强器，可以让你反弹调用多个调度）或者 [redux-batch ](https://github.com/manaflair/redux-batch)（一个存储增强器，用于处理用单个订户通知调度一系列操作）。

#### 更多信息

**讨论**

- [#125: Strategy for avoiding cascading renders（](https://github.com/reactjs/redux/issues/125)避免级联渲染的策略[）](https://github.com/reactjs/redux/issues/125)

- [#542: Idea: batching actions（](https://github.com/reactjs/redux/issues/542)想法：配料行动[）](https://github.com/reactjs/redux/issues/542)

- [#911: Batching actions（](https://github.com/reactjs/redux/issues/911)批量操作[）](https://github.com/reactjs/redux/issues/911)

- [#1813: Use a loop to support dispatching arrays（](https://github.com/reactjs/redux/pull/1813)使用循环来支持调度数组[）](https://github.com/reactjs/redux/pull/1813)

- [React Redux #263: Huge performance issue when dispatching hundreds of actions（](https://github.com/reactjs/react-redux/issues/263)调度数百个动作时性能问题巨大[）](https://github.com/reactjs/react-redux/issues/263)

**文库**

- [Redux 插件目录：存储 - 更改订阅](https://github.com/markerikson/redux-ecosystem-links/blob/master/store.md#store-change-subscriptions)将有“一个状态树”导致内存问题？分配许多动作会占用内存吗？首先，就原始内存使用而言，Redux 与其他任何 JavaScript 库没有区别。唯一的区别是所有的各种对象引用都嵌套在一个树中，而不是保存在各种独立的模型实例中，比如在 Backbone 中。其次，典型的 Redux 应用程序的内存使用可能会少于等效的 Backbone 应用程序，因为 Redux 鼓励使用普通的 JavaScript 对象和数组，而不是创建模型和集合的实例。最后，Redux 一次只保存一个状态树引用。像往常一样，不再在该树中引用的对象将被垃圾回收。Redux 不存储操作本身的历史记录。但是，Redux DevTools 确实存储了可以重播的操作，但这些操作通常只在开发过程中启用，并且不能在生产中使用。

更多信息

### **Documentation**

- 文档：异步操作

**讨论**

- [Stack Overflow: Is there any way to "commit" the state in Redux to free memory?（](http://stackoverflow.com/questions/35627553/is-there-any-way-to-commit-the-state-in-redux-to-free-memory/35634004)堆栈溢出：有没有什么办法可以在 Redux 中“提交”状态来释放内存？[）](http://stackoverflow.com/questions/35627553/is-there-any-way-to-commit-the-state-in-redux-to-free-memory/35634004)

- [Stack Overflow: Can a Redux store lead to a memory leak?（](https://stackoverflow.com/questions/39943762/can-a-redux-store-lead-to-a-memory-leak/40549594#40549594)堆栈溢出：Redux 存储会导致内存泄漏吗？[）](https://stackoverflow.com/questions/39943762/can-a-redux-store-lead-to-a-memory-leak/40549594#40549594)

- [Stack Overflow: Redux and ALL the application state（](https://stackoverflow.com/questions/42489557/redux-and-all-the-application-state/42491766#42491766)堆栈溢出：Redux 和所有的应用程序状态[）](https://stackoverflow.com/questions/42489557/redux-and-all-the-application-state/42491766#42491766)

- [Stack Overflow: Memory Usage Concern with Controlled Components（](https://stackoverflow.com/questions/44956071/memory-usage-concern-with-controlled-components?noredirect=1&lq=1)堆栈溢出：与受控组件有关的内存使用情况[）](https://stackoverflow.com/questions/44956071/memory-usage-concern-with-controlled-components?noredirect=1&lq=1)

- [Reddit: What's the best place to keep initial state?（](https://www.reddit.com/r/reactjs/comments/47m9h5/whats_the_best_place_to_keep_the_initial_state/)保持初始状态的最佳地点是什么？[）](https://www.reddit.com/r/reactjs/comments/47m9h5/whats_the_best_place_to_keep_the_initial_state/)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18