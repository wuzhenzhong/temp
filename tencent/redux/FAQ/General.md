# Redux FAQ：常规（General）

- 贡献者2人

  

## 目录

- 什么时候应该使用 Redux？

- Redux 只能与 React 一起使用吗？

- 我是否需要特定的构建工具才能使用 Redux？

## 一般

### 什么时候应该使用 Redux？

作为 React 早期贡献者之一的 Pete Hunt 说：

> 你会知道什么时候需要 Flux。如果你不确定是否需要，你则不需要。

同样，Redux 的创始人之一Dan Abramov说：

> 我想修改这个：在使用 vanilla React 之前，请不要使用 Redux。

一般来说，当你有合理数量的数据随时间变化时，使用 Redux，你需要一个单一的事实来源，并且你发现像把所有东西放在顶层React组件的状态这样的方法已经不够了。

但是，了解使用 Redux 带来的折衷也很重要。它的设计不是编写代码的最短或最快的方式。它旨在帮助回答“何时进行了某种状态改变以及数据从何而来？”这个问题，并且具有可预测的行为。它通过要求您遵循应用程序中的特定约束来实现：将应用程序的状态存储为普通数据，将更改作为普通对象进行描述，并使用不变应用更新的纯函数处理这些更改。这通常是关于“样板”的投诉的来源。这些约束需要开发人员付出努力，但也需要开放一些额外的可能性（例如存储持久性和同步）。

如果您刚刚学习 React，您应该首先关注 React，然后在您更好地理解 React 以及 Redux 如何适合您的应用程序时查看 Redux。

最后，Redux只是一个工具。这是一个很棒的工具，使用它有一些很好的理由，但也有你可能不想使用它的原因。对工具做出明智的决策，并理解每个决策所涉及的权衡。

#### 更多信息

**文档**

- 介绍：动机

### **文档**

- [React How-To](https://github.com/petehunt/react-howto)

- [You Might Not Need Redux（你可能不需要Redux）](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

- [The Case for Flux（Flux的例子）](https://medium.com/swlh/the-case-for-flux-379b7d1982c6)

- [Some Reasons Why Redux is Useful in a React App（](https://www.fullstackreact.com/articles/redux-with-mark-erikson/)为什么Redux在React App中很有用的一些原因[）](https://www.fullstackreact.com/articles/redux-with-mark-erikson/)

**讨论**

- [Twitter: Don't use Redux until...（](https://twitter.com/dan_abramov/status/699241546248536064)Twitter：直到......才使用Redux[）](https://twitter.com/dan_abramov/status/699241546248536064)

- [Twitter: Redux is designed to be predictable, not concise（](https://twitter.com/dan_abramov/status/733742952657342464)Twitter：Redux的设计是可预测的，而不是简洁[）](https://twitter.com/dan_abramov/status/733742952657342464)

- [Twitter: Redux is useful to eliminate deep prop passing（](https://twitter.com/dan_abramov/status/732912085840089088)Twitter：Redux有助于消除深度道具传球[）](https://twitter.com/dan_abramov/status/732912085840089088)

- [Twitter: Don't use Redux unless you're unhappy with local component state（](https://twitter.com/dan_abramov/status/725089243836588032)Twitter：除非您对本地组件状态不满意，否则不要使用Redux[）](https://twitter.com/dan_abramov/status/725089243836588032)

- [Twitter: You don't need Redux if your data never changes（](https://twitter.com/dan_abramov/status/737036433215610880)Twitter：如果你的数据永不改变，你不需要 Redux[）](https://twitter.com/dan_abramov/status/737036433215610880)

- [Twitter: If your reducer looks boring, don't use redux（](https://twitter.com/dan_abramov/status/802564042648944642)Twitter：如果你的 reducer 看起来很无聊，不要使用 redux[）](https://twitter.com/dan_abramov/status/802564042648944642)

- [Reddit: You don't need Redux if your app just fetches something on a single page（](https://www.reddit.com/r/reactjs/comments/5exfea/feedback_on_my_first_redux_app/dagglqp/)Reddit：如果您的应用只是在单个页面上获取某些内容，则不需要 Redux[）](https://www.reddit.com/r/reactjs/comments/5exfea/feedback_on_my_first_redux_app/dagglqp/)

- [Stack Overflow: Why use Redux over Facebook Flux?（](http://stackoverflow.com/questions/32461229/why-use-redux-over-facebook-flux)堆栈溢出：为什么在Facebook上使用Redux通量？[）](http://stackoverflow.com/questions/32461229/why-use-redux-over-facebook-flux)

- [Stack Overflow: Why should I use Redux in this example?（](http://stackoverflow.com/questions/35675339/why-should-i-use-redux-in-this-example)堆栈溢出：为什么我应该在这个例子中使用Redux？[）](http://stackoverflow.com/questions/35675339/why-should-i-use-redux-in-this-example)

- [Stack Overflow: What could be the downsides of using Redux instead of Flux?（](http://stackoverflow.com/questions/32021763/what-could-be-the-downsides-of-using-redux-instead-of-flux)堆栈溢出：使用 Redux 而不是 Flux 的缺点是什么？[）](http://stackoverflow.com/questions/32021763/what-could-be-the-downsides-of-using-redux-instead-of-flux)

- [Stack Overflow: When should I add Redux to a React app?（](http://stackoverflow.com/questions/36631761/when-should-i-add-redux-to-a-react-app)堆栈溢出：我应该何时将Redux添加到React应用程序？[）](http://stackoverflow.com/questions/36631761/when-should-i-add-redux-to-a-react-app)

- [Stack Overflow: Redux vs plain React?（](http://stackoverflow.com/questions/39260769/redux-vs-plain-react/39261546#39261546)堆栈溢出：Redux vs普通React？[）](http://stackoverflow.com/questions/39260769/redux-vs-plain-react/39261546#39261546)

- [Twitter: Redux is a platform for developers to build customized state management with reusable things（](https://twitter.com/acemarke/status/793862722253447168)Twitter：Redux是开发人员用可重用的东西构建定制状态管理的平台[）](https://twitter.com/acemarke/status/793862722253447168)

### Redux 只能与 React 一起使用吗？

Redux 可以用作任何 UI 层的数据存储。最常见的用法是使用 React 和 React Native，但是有 Angular，Angular 2，Vue，Mithril 等可用的绑定。Redux 只是提供了一个可以被任何其他代码使用的订阅机制。也就是说，与声明式视图实现相结合时，它可以从状态变化中推断出 UI 更新，如 React 或可用的类似库之一，这是非常有用的。

### 我是否需要特定的构建工具才能使用 Redux？

Redux 最初是用 ES6 编写的，并用 Webpack 和 Babel 编译生成 ES5。无论您的 JavaScript 构建过程如何，您都应该可以使用。Redux 还提供了 UMD 构建，可以在没有任何构建过程的情况下直接使用。[counter-vanilla](https://github.com/reactjs/redux/tree/master/examples/counter-vanilla)案例展示了终极版基本 ES5 使用包括作为`<script>`标记。正如相关的请求说：

> 新的 Counter Vanilla 示例旨在消除 Redux 需要 Webpack ，React，热重载，传奇，动作创建者，常量，Babel，npm，CSS模块，装饰器，流利的拉丁语，Egghead订阅，博士学位或超越期望OWL水平。不，它只是HTML，一些手工`<script>`标记和简单的旧DOM操作。请享用！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18