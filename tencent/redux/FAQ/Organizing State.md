# Redux FAQ：组织形态（Organizing State）

- 贡献者2人

  

## 目录

- 我必须将我的所有状态都放入 Redux 吗？我应该使用 React 的 setState（） 吗？

- 我可以在我的商店状态中放入函数，承诺或其他不可序列化的项目吗？

- 如何在我的状态下组织嵌套或重复的数据？

## 组织状况

### 我必须将我的所有状态都放入 Redux 吗？我应该使用 React `setState()`吗？

对此没有“正确的”答案。有些用户更喜欢在 Redux 中保留每一个数据片段，以便始终保持其应用程序的完全可序列化和受控版本。其他人更喜欢在组件的内部状态内保持非关键或UI状态，例如“当前打开的下拉列表”。

***使用本地组件状态很好***。作为一名开发人员，*您的*工作是确定组成您的应用程序的状态以及每个州应该在哪里生活。找到适合你的平衡点，并与之配合。

确定应将什么类型的数据放入 Redux 的一些常用经验法则是：

- 应用程序的其他部分是否关心这些数据？

- 你需要能够根据这些原始数据创建更多派生数据吗？

- 是否使用相同的数据来驱动多个组件？

- 能够将这种状态恢复到某个特定时间点（例如，时间旅行调试）对您来说是否有价值？

- 你想缓存数据吗（例如，如果它已经存在，而不是重新请求它，使用什么状态）？

有许多社区软件包实现了在 Redux 存储中存储每个组件状态的各种方法，例如 [redux-ui](https://github.com/tonyhb/redux-ui)，[redux-component](https://github.com/tomchentw/redux-component)，[redux-react-local ](https://github.com/threepointone/redux-react-local)等等。也可以将 Redux 的原理和减速器的概念应用到更新本地组件状态的任务中`this.setState( (previousState) => reducer(previousState, someAction))`。

#### 更多信息

**文章**

- [You Might Not Need Redux（](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)你可能不需要Redux[）](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

- [Finding `state`'s place with React and Redux（](https://medium.com/@adamrackis/finding-state-s-place-with-react-and-redux-e9a586630172)用 React 和 Redux 查找状态的位置[）](https://medium.com/@adamrackis/finding-state-s-place-with-react-and-redux-e9a586630172)

- [A Case for setState（](https://medium.com/@zackargyle/a-case-for-setstate-1f1c47cd3f73)一个setState的例子[）](https://medium.com/@zackargyle/a-case-for-setstate-1f1c47cd3f73)

- [How to handle state in React: the missing FAQ（](https://medium.com/react-ecosystem/how-to-handle-state-in-react-6f2d3cd73a0c)如何处理React中的状态：缺少的常见问题[）](https://medium.com/react-ecosystem/how-to-handle-state-in-react-6f2d3cd73a0c)

- [Where to Hold React Component Data: state, store, static, and this（](https://medium.freecodecamp.com/where-do-i-belong-a-guide-to-saving-react-component-data-in-state-store-static-and-this-c49b335e2a00)何处保留React组件数据：状态，存储，静态和this[）](https://medium.freecodecamp.com/where-do-i-belong-a-guide-to-saving-react-component-data-in-state-store-static-and-this-c49b335e2a00)

- [The 5 Types of React Application State（](http://jamesknelson.com/5-types-react-application-state/)5种反应申请状态[）](http://jamesknelson.com/5-types-react-application-state/)

**讨论**

- [#159: Investigate using Redux for pseudo-local component state（](https://github.com/reactjs/redux/issues/159)使用Redux调查伪本地组件状态[）](https://github.com/reactjs/redux/issues/159)

- [#1098: Using Redux in reusable React component（](https://github.com/reactjs/redux/issues/1098)在可重用的React组件中使用Redux[）](https://github.com/reactjs/redux/issues/1098)

- [#1287: How to choose between Redux's store and React's state?（](https://github.com/reactjs/redux/issues/1287)如何选择Redux的商店和React的状态？[）](https://github.com/reactjs/redux/issues/1287)

- [#1385: What are the disadvantages of storing all your state in a single immutable atom?（](https://github.com/reactjs/redux/issues/1385)将所有状态存储在单个不可变原子中的缺点是什么？[）](https://github.com/reactjs/redux/issues/1385)

- [Twitter: Should I keep something in React component state?（](https://twitter.com/dan_abramov/status/749710501916139520)Twitter：我应该保留一些React组件状态吗？[）](https://twitter.com/dan_abramov/status/749710501916139520)

- [Twitter: Using a reducer to update a component（](https://twitter.com/dan_abramov/status/736310245945933824)Twitter：使用reducer更新组件[）](https://twitter.com/dan_abramov/status/736310245945933824)

- [React Forums: Redux and global state vs local state（](https://discuss.reactjs.org/t/redux-and-global-state-vs-local-state/4187)React 论坛：Redux 和全局状态与本地状态[）](https://discuss.reactjs.org/t/redux-and-global-state-vs-local-state/4187)

- [Reddit: "When should I put something into my Redux store?"（](https://www.reddit.com/r/reactjs/comments/4w04to/when_using_redux_should_all_asynchronous_actions/d63u4o8)Reddit：“我什么时候应该把什么东西放进我的 Redux 商店？”[）](https://www.reddit.com/r/reactjs/comments/4w04to/when_using_redux_should_all_asynchronous_actions/d63u4o8)

- [Stack Overflow: Why is state all in one place, even state that isn't global?（](http://stackoverflow.com/questions/35664594/redux-why-is-state-all-in-one-place-even-state-that-isnt-global)堆栈溢出：为什么状态在一个地方，甚至是不是全局的状态？[）](http://stackoverflow.com/questions/35664594/redux-why-is-state-all-in-one-place-even-state-that-isnt-global)

- [Stack Overflow: Should all component state be kept in Redux store?（](http://stackoverflow.com/questions/35328056/react-redux-should-all-component-states-be-kept-in-redux-store)堆栈溢出：是否所有组件状态都保存在 Redux 存储中？[）](http://stackoverflow.com/questions/35328056/react-redux-should-all-component-states-be-kept-in-redux-store)

**文库**

- [Redux 插件目录：组件状态](https://github.com/markerikson/redux-ecosystem-links/blob/master/component-state.md)我可以将函数，承诺或其他不可序列化的项目放入商店状态吗？强烈建议您只将简单的可序列化对象，数组和基元放入商店。将不可序列化的商品插入商店在*技术上是*可行的，但是这样做可能会打破商店内容的坚持和补水的能力，并干扰时间旅行调试。如果您对持久性和时间等事情没有问题-travel调试可能无法按预期工作，那么非常欢迎您将不可序列化的项目放入Redux存储区。最终，这是*你的*应用程序，以及如何实现它取决于你。与 Redux 的其他许多事情一样，只要确保明白涉及哪些折衷。更多信息

### **讨论**

- [#1248: Is it ok and possible to store a react component in a reducer?（](https://github.com/reactjs/redux/issues/1248)可以将反应组分储存在减速器中吗？[）](https://github.com/reactjs/redux/issues/1248)

- [#1279: Have any suggestions for where to put a Map Component in Flux?（](https://github.com/reactjs/redux/issues/1279)对于在 Flux 中放置地图组件有什么建议？[）](https://github.com/reactjs/redux/issues/1279)

- [#1390: Component Loading（](https://github.com/reactjs/redux/issues/1390)组件加载[）](https://github.com/reactjs/redux/issues/1390)

- [#1407: Just sharing a great base class（](https://github.com/reactjs/redux/issues/1407)只是分享一个伟大的基础层级[）](https://github.com/reactjs/redux/issues/1407)

- [#1793: React Elements in Redux State（](https://github.com/reactjs/redux/issues/1793)在 Redux 状态中反应元素[）](https://github.com/reactjs/redux/issues/1793)

### 如何在我的状态下组织嵌套或重复的数据？

带 ID，嵌套或关系的数据通常应以“规范化”的方式存储：每个对象应存储一次，以 ID 为键，其他引用该对象的对象应仅存储ID而不是整个对象的副本。将商店的某些部分视为数据库可能会有所帮助，每种商品类型都有单独的“表格”。像 [normalizr ](https://github.com/paularmstrong/normalizr)和 [redux-orm ](https://github.com/tommikaikkonen/redux-orm)这样的库可以提供管理规范化数据的帮助和抽象。

#### 更多信息

**文档**

- Advanced: Async Actions（高级：异步操作）

- Examples: Real World example（示例：真实世界的例子）

- Recipes: Structuring Reducers - Prerequisite Concepts（构建Reducers - 先决条件的概念）

- Recipes: Structuring Reducers - Normalizing State Shape（构造减速器 - 规范化状态形状）

- [Examples: Tree View（](https://github.com/reactjs/redux/tree/master/examples/tree-view)示例：树视图[）](https://github.com/reactjs/redux/tree/master/examples/tree-view)

**文章**

- [High-Performance Redux（](http://somebody32.github.io/high-performance-redux/)高性能 Redux[）](http://somebody32.github.io/high-performance-redux/)

- [Querying a Redux Store（](https://medium.com/@adamrackis/querying-a-redux-store-37db8c7f3b0f)查询 Redux 商店[）](https://medium.com/@adamrackis/querying-a-redux-store-37db8c7f3b0f)

**讨论**

- [#316: How to create nested reducers?（](https://github.com/reactjs/redux/issues/316)如何创建嵌套减速器？[）](https://github.com/reactjs/redux/issues/316)

- [#815: Working with Data Structures（](https://github.com/reactjs/redux/issues/815)使用数据结构[）](https://github.com/reactjs/redux/issues/815)

- [#946: Best way to update related state fields with split reducers?（](https://github.com/reactjs/redux/issues/946)使用拆分式缩减器更新相关状态字段的最佳方法？[）](https://github.com/reactjs/redux/issues/946)

- [#994: How to cut the boilerplate when updating nested entities?（](https://github.com/reactjs/redux/issues/994)如何在更新嵌套实体时剪切样板文件？[）](https://github.com/reactjs/redux/issues/994)

- [#1255: Normalizr usage with nested objects in React/Redux（](https://github.com/reactjs/redux/issues/1255)Normalize使用React/Redux中的嵌套对象[）](https://github.com/reactjs/redux/issues/1255)

- [#1269: Add tree view example（](https://github.com/reactjs/redux/pull/1269)添加树视图示例[）](https://github.com/reactjs/redux/pull/1269)

- [#1824: Normalising state and garbage collection（](https://github.com/reactjs/redux/issues/1824#issuecomment-228585904)规范化状态和垃圾收集[）](https://github.com/reactjs/redux/issues/1824#issuecomment-228585904)

- [Twitter: state shape should be normalized（](https://twitter.com/dan_abramov/status/715507260244496384)Twitter：状态应该正常化[）](https://twitter.com/dan_abramov/status/715507260244496384)

- [Stack Overflow: How to handle tree-shaped entities in Redux reducers?（](http://stackoverflow.com/questions/32798193/how-to-handle-tree-shaped-entities-in-redux-reducers)堆栈溢出：如何处理 Redux reducer 中的树形实体？[）](http://stackoverflow.com/questions/32798193/how-to-handle-tree-shaped-entities-in-redux-reducers)

- [Stack Overflow: How to optimize small updates to props of nested components in React + Redux?（](http://stackoverflow.com/questions/37264415/how-to-optimize-small-updates-to-props-of-nested-component-in-react-redux)堆栈溢出：如何优化 React + Redux 中嵌套组件的小道具更新？[）](http://stackoverflow.com/questions/37264415/how-to-optimize-small-updates-to-props-of-nested-component-in-react-redux)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18