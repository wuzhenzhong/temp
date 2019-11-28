# 现有技术（Prior Art）

- 贡献者1人

  

Redux 具有混合继承。它类似于一些模式和技术，但在重要方面也与它们不同。我们将探索下面的一些相似点和差异。

### Flux

Redux 可以被认为是 [Flux ](https://facebook.github.io/flux/)实现吗？

[是](https://twitter.com/fisherwebdev/status/616278911886884864)，[不是](https://twitter.com/andrestaltz/status/616270755605708800)。

（别担心，[Flux创作者](https://twitter.com/jingc/status/616608251463909376)，如果这就是你想知道的一切。）

Redux 受到 Flux 几个重要特质的启发。与Flux类似，Redux 规定您将模型更新逻辑集中在应用程序的某一层（Flux 中的“存储”，Redux 中的 “reducer” ）。不是让应用程序代码直接变更数据，而是告诉您将每个变异描述为一个称为“动作”的普通对象。

与 Flux 不同，**Redux 没有 Dispatcher 的概念**。这是因为它依赖于纯函数而不是事件发射器，纯函数很容易编写，不需要额外的实体来管理它们。根据您对 Flux 的看法，您可能会将其视为偏差或实施细节。Flux 经常被[描述为`(state, action) => state`](https://speakerdeck.com/jmorrell/jsconf-uy-flux-those-who-forget-the-past-dot-dot-dot-1)。从这个意义上说，Redux 对于 Flux 架构是真实的，但是由于纯粹的功能，它变得更加简单。

Flux 的另一个重要区别是，**Redux 假定您从不改变数据**。你可以在你的状态下使用普通对象和数组，但是强烈建议不要在 reducer 中对它们进行变异。你应该总是返回一个新对象，这对于对象扩展运算符提议来说很简单，或者像一个像 [Immutable ](https://facebook.github.io/immutable-js)这样的库。

虽然在技术上是**可能的**，以[写不纯的减速器](https://github.com/reactjs/redux/issues/328#issuecomment-125035516)是变异的数据表现角落的情况下，我们积极鼓励你这样做。诸如时间旅行，录制/回放或热重载等开发功能将会中断。此外，似乎不变性在大多数实际应用中都会造成性能问题，因为正如[Om所](https://github.com/omcljs/om)表明的那样，即使您错过了对象分配，仍然可以避免昂贵的重新渲染和重新计算，因为您确切知道是什么由于减速器纯度而改变。

### Elm

[Elm ](http://elm-lang.org/)是由Haskell启发并由 [Evan Czaplicki ](https://twitter.com/czaplic)创建的函数式编程语言。它强制执行[“模型视图更新”体系结构](https://github.com/evancz/elm-architecture-tutorial/)，其更新具有以下签名：`(action, state) => state`。Elm “更新程序”与 Redux中的还原程序具有相同的用途。

与Redux不同，Elm是一种语言，所以它可以从许多方面受益，如强制纯度，静态类型，开箱即用的不变性和模式匹配（使用`case`表达式）。即使你不打算使用Elm，你也应该阅读关于Elm架构的知识，并且玩它。有一个有趣的[JavaScript库操作实现类似的想法](https://github.com/paldepind/noname-functional-frontend-framework)。我们应该在Redux上寻找灵感！我们可以更接近Elm的静态类型的一种方式是[使用像Flow这样的渐进式打字解决方案](https://github.com/reactjs/redux/issues/290)。

**Immutable**

[Immutable ](https://facebook.github.io/immutable-js)是一个实现持久数据结构的 JavaScript 库。它是高性能的，并有一个惯用的 JavaScript API。

不可变且最相似的库与Redux正交。随意一起使用它们！

**Redux 不关心** **你如何** **存储状态 - 它可以是一个普通对象，一个不可变对象或其他任何东西。**您可能需要用于编写通用应用程序的序列化机制，并从服务器获取其状态，但**除此之外，只要支持不变性**，就可以使用任何数据存储库。例如，使用 Backbone for Redux 状态没有意义，因为 Backbone 模型是可变的。

请注意，即使您的不可变库支持游标，您也不应在Redux应用程序中使用它们。整个状态树应该被认为是只读的，你应该使用 Redux 来更新状态并订阅更新。因此，通过光标书写对 Redux来说没有意义。**如果你唯一的游标用例是将状态树从UI树中分离出来并逐渐改进游标，那么你应该看看选择器。**选择器是可组合的 getter 函数。请参阅[重新选择](http://github.com/faassen/reselect)组合选择器的一个非常好的和简洁的实现。

### Baobab

[Baobab ](https://github.com/Yomguithereal/baobab)是另一个流行的库，它实现了用于更新普通 JavaScript 对象的不可变 API 。虽然您可以在 Redux 中使用它，但将它们一起使用几乎没有什么好处。

Baobab 提供的大部分功能都与使用游标更新数据有关，但 Redux 强制更新数据的唯一方法是发送操作。因此，他们以不同的方式解决相同的问题，而不是相互补充。

与 Immutable 不同的是，Baobab 还没有在底层实现任何特殊的高效数据结构，所以你不会真的赢得与 Redux 一起使用的任何东西。在这种情况下使用简单对象更容易。

### RxJS

[RxJS](https://github.com/ReactiveX/RxJS)是管理异步应用程序复杂性的极好方法。实际上，[人们努力创建一个模型将人机交互作为相互依存的可观察模型](http://cycle.js.org/)。

将 Redux 与 RxJS 一起使用是否有意义？当然！他们一起工作很棒。例如，将 Redux 商店作为可观察对象很容易：

```javascript
function toObservable(store) {
  return {
    subscribe({ next }) {
      const unsubscribe = store.subscribe(() => next(store.getState()))
      next(store.getState())
      return { unsubscribe }
    }
  }
}
```

同样，您可以编写不同的异步流，以在将它们提供给它们之前将它们转换为动作`store.dispatch()`。

问题是：如果您已经使用Rx，您是否真的需要Redux？也许不会。[在Rx中重新实现Redux](https://github.com/jas-chen/rx-redux)并不难。有人说这是一个使用Rx `.scan()`方法的双线程。它可能很好！

如果您有疑问，请查看 Redux 源代码（这里没有太多内容）以及它的生态系统（例如[开发人员工具](https://github.com/gaearon/redux-devtools)）。如果你不关心它，并希望一直使用反应式数据流，那么你可能想要探索像 [Cycle ](http://cycle.js.org/)这样的事情，或者甚至将它与 Redux 结合起来。让我们知道怎么回事！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18