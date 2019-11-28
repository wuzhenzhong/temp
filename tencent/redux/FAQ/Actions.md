# Redux FAQ：操作（Actions）

- 贡献者2人

  

## 目录

- 为什么要输入一个字符串，或者至少可以序列化？为什么我的动作类型是常量？

- 减速器和动作之间总是存在一对一的映射关系吗？

- 我如何表示“副作用”，如AJAX调用？为什么我们需要像“action creators”，“thunk”和“middleware”这样的东西来做异步行为？

- 我应该从一个动作创作者连续发送多个动作吗？

## 操作

### 为什么`type`应该是一个字符串，或者至少是可序列化的？为什么我的动作类型是常量？

与状态一样，可序列化操作启用 Redux 的几个定义功能，例如时间旅行调试，以及记录和重放操作。使用类似于a `Symbol`的`type`值或使用`instanceof`检查操作本身会破坏这一点。字符串是可序列化的，并且易于自我描述，因此是更好的选择。请注意，这*是*好的，如果动作是供中间件使用使用符号，承诺，或其他非序列值的操作。动作只需要在实际到达商店时被序列化并传递给减速器。

由于性能原因，我们无法可靠地执行可序列化操作，因此 Redux 仅检查每个操作是否为普通对象，并且该操作是否`type`已定义。其余的由您决定，但是您可能会发现保持所有序列化操作有助于调试和重现问题。

封装和集中常用代码段是编程中的一个关键概念。尽管无处不在手动创建动作对象并手动编写每个`type`值，但定义可重用常量使维护代码更加容易。如果你把常量放在一个单独的文件中，你可以[检查你的`import`语句是否有错别字，](https://www.npmjs.com/package/eslint-plugin-import)这样你就不会意外地使用错误的字符串。

#### 更多信息

**文档**

- 减少样板

### **讨论**

- [#384: 建议使用过去式来命名Action常量](https://github.com/reactjs/redux/issues/384)

- [#628:用较少的样板创建简单动作的解决方案](https://github.com/reactjs/redux/issues/628)



- [#1024: 建议：声明性减速器](https://github.com/reactjs/redux/issues/1024)

- [#1167: 没有开关的减速器](https://github.com/reactjs/redux/issues/1167)

- [堆栈溢出：为什么在Redux中需要'Actions'作为数据？](http://stackoverflow.com/q/34759047/62937)

- [堆栈溢出： Redux中常量的点是什么？](http://stackoverflow.com/q/34965856/62937)

### 减速器和动作之间总是存在一对一的映射关系吗？

不是的。我们建议您编写独立的小型还原器函数，每个函数负责更新特定的状态片。我们称这种模式为“减速器组成”。一个给定的行动可以被全部，部分或全部处理。这使得组件与实际数据更改分离，因为一个动作可能会影响状态树的不同部分，并且组件不需要知道这一点。一些用户确实选择将它们更紧密地绑定在一起，比如“ducks”文件结构，但默认情况下肯定没有一对一的映射，并且只要你觉得你想要的时候你应该跳出这样的范式在许多减速器中处理动作。

#### 更多信息

**文档**

- 基础知识：减速器

- Recipes: 构造减速器

**讨论**

- Twitter: most common Redux misconception

- [#1167: Reducer without switch](https://github.com/reactjs/redux/issues/1167)

- [Reduxible #8: Reducers 和 action creators不是一对一的映射](https://github.com/reduxible/reduxible/issues/8)

- [Stack Overflow堆栈溢出：我可以在没有Redux Thunk中间件的情况下调度多个操作吗？](http://stackoverflow.com/questions/35493352/can-i-dispatch-multiple-actions-without-redux-thunk-middleware/35642783)

### 我如何表示“side effects”，如 AJAX 调用？为什么我们需要像“**action creators**”，“thunk”和“middleware”这样的东西来做异步行为？

这是一个漫长而复杂的话题，关于如何组织代码以及应该采用什么方法提出了各种各样的意见。

任何有意义的 Web 应用程序都需要执行复杂的逻辑，通常包括异步工作，例如制作 AJAX 请求。该代码不再纯粹是其输入的函数，并且与外界的交互被称为“side effects”

Redux 的灵感来自于函数式编程，开箱即用，无法执行副作用。特别是减速机函数*必须*始终是纯粹的函数`(state, action) => newState`。但是，Redux 的中间件可以拦截已分派的操作并在其周围添加其他复杂行为，包括副作用。

一般来说，Redux 建议具有副作用的代码应该是动作创建过程的一部分。尽管该逻辑*可以*在 UI 组件内执行，但通常将该逻辑抽取为可重用的函数是有意义的，以便可以从多个位置调用相同的逻辑 - 换句话说，就是动作创建器函数。

最简单也是最常用的方法是添加 [Redux Thunk ](https://github.com/gaearon/redux-thunk)中间件，该中间件可让您为动作创建者编写更复杂和异步的逻辑。另一个广泛使用的方法是 [Redux Saga](https://github.com/yelouafi/redux-saga)，它允许您使用生成器编写更多的同步代码，并且可以在Redux应用程序中充当“后台线程”或“守护进程”。另一种方法是 [Redux Loop](https://github.com/raisemarketplace/redux-loop)，它通过允许你的 reducer 声明副作用来响应状态变化并使它们分开执行来颠倒过程。除此之外，还有*许多*其他社区开发的图书和想法，每个都有自己的副作用应该如何管理。

#### 更多信息

**文档**

- 高级: 异步操作

- 高级：异步流程

- 高级：中间件

文章

- [Redux Side-Effects and You](https://medium.com/@fward/redux-side-effects-and-you-66f2e0842fc3)

- [Pure functionality and side effects in Redux（](http://blog.hivejs.org/building-the-ui-2/)Redux中的纯功能和副作用[）](http://blog.hivejs.org/building-the-ui-2/)

- [From Flux to Redux: Async Actions the easy way（](http://danmaz74.me/2015/08/19/from-flux-to-redux-async-actions-the-easy-way/)从Flux到Redux：异步操作是一种简单的方法[）](http://danmaz74.me/2015/08/19/from-flux-to-redux-async-actions-the-easy-way/)

- [React/Redux Links: "Redux Side Effects" category（](https://github.com/markerikson/react-redux-links/blob/master/redux-side-effects.md)React / Redux链接：“Redux副作用”类别[）](https://github.com/markerikson/react-redux-links/blob/master/redux-side-effects.md)

- [Gist: Redux-Thunk examples（](https://gist.github.com/markerikson/ea4d0a6ce56ee479fe8b356e099f857e)Gist：Redux-Thunk示例[）](https://gist.github.com/markerikson/ea4d0a6ce56ee479fe8b356e099f857e)

**讨论**

- [#291: Trying to put API calls in the right place（](https://github.com/reactjs/redux/issues/291)试图将API调用放在正确的位置[）](https://github.com/reactjs/redux/issues/291)

- [#455: Modeling side effects（](https://github.com/reactjs/redux/issues/455)建模副作用[）](https://github.com/reactjs/redux/issues/455)

- [#533: Simpler introduction to async action creators（](https://github.com/reactjs/redux/issues/533)简单介绍异步动作创建者[）](https://github.com/reactjs/redux/issues/533)

- [#569: Proposal: API for explicit side effects（](https://github.com/reactjs/redux/pull/569)建议：明确的副作用 API[）](https://github.com/reactjs/redux/pull/569)

- [#1139: An alternative side effect model based on generators and sagas（](https://github.com/reactjs/redux/issues/1139)基于加速器和sagas的替代副作用模型[）](https://github.com/reactjs/redux/issues/1139)

- [Stack Overflow: Why do we need middleware for async flow in Redux?（](http://stackoverflow.com/questions/34570758/why-do-we-need-middleware-for-async-flow-in-redux)堆栈溢出：为什么我们需要Redux中的异步流中间件？[）](http://stackoverflow.com/questions/34570758/why-do-we-need-middleware-for-async-flow-in-redux)

- [Stack Overflow: How to dispatch a Redux action with a timeout?（](http://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559)堆栈溢出：如何使用超时分派 Redux 动作？[）](http://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559)

- [Stack Overflow: Where should I put synchronous side effects linked to actions in redux?（](http://stackoverflow.com/questions/32982237/where-should-i-put-synchronous-side-effects-linked-to-actions-in-redux/33036344)堆栈溢出：我应该把哪些同步副作用与redux中的动作关联起来？[）](http://stackoverflow.com/questions/32982237/where-should-i-put-synchronous-side-effects-linked-to-actions-in-redux/33036344)

- [Stack Overflow: How to handle complex side-effects in Redux?（](http://stackoverflow.com/questions/32925837/how-to-handle-complex-side-effects-in-redux/33036594)堆栈溢出：如何在Redux中处理复杂的副作用？[）](http://stackoverflow.com/questions/32925837/how-to-handle-complex-side-effects-in-redux/33036594)

- [Stack Overflow: How to unit test async Redux actions to mock ajax response（](http://stackoverflow.com/questions/33011729/how-to-unit-test-async-redux-actions-to-mock-ajax-response/33053465)堆栈溢出：如何单元测试异步Redux操作来模拟ajax响应[）](http://stackoverflow.com/questions/33011729/how-to-unit-test-async-redux-actions-to-mock-ajax-response/33053465)

- [Stack Overflow: How to fire AJAX calls in response to the state changes with Redux?（](http://stackoverflow.com/questions/35262692/how-to-fire-ajax-calls-in-response-to-the-state-changes-with-redux/35675447)堆栈溢出：如何触发AJAX调用来响应Redux的状态更改？[）](http://stackoverflow.com/questions/35262692/how-to-fire-ajax-calls-in-response-to-the-state-changes-with-redux/35675447)

- [Reddit: Help performing Async API calls with Redux-Promise Middleware.（](https://www.reddit.com/r/reactjs/comments/469iyc/help_performing_async_api_calls_with_reduxpromise/)Reddit：帮助用Redux-Promise中间件执行异步API调用。[）](https://www.reddit.com/r/reactjs/comments/469iyc/help_performing_async_api_calls_with_reduxpromise/)

- [Twitter: possible comparison between sagas, loops, and other approaches（](https://twitter.com/dan_abramov/status/689639582120415232)Twitter：传奇，循环和其他方法之间的可能比较[）](https://twitter.com/dan_abramov/status/689639582120415232)

### 我应该从一个动作创作者连续发送多个动作吗？

没有关于如何构建你的行为的具体规则。使用像 Redux Thunk 这样的异步中间件当然可以实现诸如在一行中分派多个不同但相关的动作，分派动作以表示 AJAX 请求的进展，基于状态有条件地分派动作，或者甚至分派动作并立即检查更新的状态之后。

一般来说，要问这些行为是相关的但是独立的，还是应该实际表现为一个行为。对自己的情况做些有意义的事情，但试着平衡减速器的可读性和动作日志的可读性。例如，包含整个新状态树的操作将使您的 reducer 成为一行，但缺点是您现在没有关于变化发生的*原因*的历史记录，所以调试变得非常困难。另一方面，如果你在一个循环中发出动作以保持细化，这表明你可能想引入一个以不同方式处理的新动作类型。

尽量避免在您关心性能的地方连续多次同步调度。还有一些插件和方法可以批量发送。

#### 更多信息

**文档**

- FAQ: 性能 - 减少更新事件

### **讨论**

- [#597: Valid to dispatch multiple actions from an event handler?（](https://github.com/reactjs/redux/issues/597)有效从事件处理程序分派多个操作？[）](https://github.com/reactjs/redux/issues/597)

- [#959: Multiple actions one dispatch?（](https://github.com/reactjs/redux/issues/959)多动作一派遣？[）](https://github.com/reactjs/redux/issues/959)

- [Stack Overflow: Should I use one or several action types to represent this async action?（](http://stackoverflow.com/questions/33637740/should-i-use-one-or-several-action-types-to-represent-this-async-action/33816695)堆栈溢出：我应该使用一种或多种动作类型来表示这种异步动作吗？[）](http://stackoverflow.com/questions/33637740/should-i-use-one-or-several-action-types-to-represent-this-async-action/33816695)

- [Stack Overflow: Do events and actions have a 1:1 relationship in Redux?（](http://stackoverflow.com/questions/35406707/do-events-and-actions-have-a-11-relationship-in-redux/35410524)堆栈溢出：事件和动作在Redux中有1：1的关系吗？[）](http://stackoverflow.com/questions/35406707/do-events-and-actions-have-a-11-relationship-in-redux/35410524)

- [Stack Overflow: Should actions be handled by reducers to related actions or generated by action creators themselves?（](http://stackoverflow.com/questions/33220776/should-actions-like-showing-hiding-loading-screens-be-handled-by-reducers-to-rel/33226443#33226443)堆栈溢出：行动是否应由减法者处理相关行为或由行动创造者自己产生？[）](http://stackoverflow.com/questions/33220776/should-actions-like-showing-hiding-loading-screens-be-handled-by-reducers-to-rel/33226443#33226443)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18