# Redux FAQ

- 贡献者1人

  

## 目录

- General
  - 什么时候应该使用 Redux？
  - Redux 只能与 React一起使用吗？
  - 我是否需要特定的构建工具才能使用 Redux？
- Reducers
  - 我如何在两个减速器之间共享状态？我必须使用联合收割机吗？
  - 我是否必须使用 switch 语句来处理操作？
- Organizing State
  - 我必须将我的所有状态都放入Redux吗？我应该使用 React 的 setState（）吗？
  - 我可以在我的商店状态中放入函数，承诺或其他不可序列化的项目吗？
  - 如何在我的状态下组织嵌套或重复的数据？
- Store Setup
  - 可以或应该创建多个商店？我可以直接导入我的商店，并在组件中使用它吗？
  - 我的商店增强器中有多个中间件链可以吗？下一步和中间件功能的调度有什么区别？
  - 我如何只订阅州的一部分？我是否可以将订单中的分派操作作为订阅的一部分？
- Actions
  - 为什么要输入一个字符串，或者至少可以序列化？为什么我的动作类型是常量？
  - 减速器和动作之间总是存在一对一的映射关系吗？
  - 我如何表示“副作用”，如 AJAX 调用？为什么我们需要像“行动创造者”，“thunk”和“middleware”这样的东西来做异步行为？
  - 我应该从一个动作创作者连续发送多个动作吗？
- Immutable Data
  - 不变性有什么好处？
  - Redux 为什么要求不变性？
  - 我必须使用 Immutable.JS 吗？
  - 使用 ES6 进行不可变操作有什么问题？
- **Using Immutable.JS with Redux**

```js
- [Why should I use an immutable-focused library such as Immutable.JS?](recipes/usingimmutablejs#why-use-immutable-library)
- [Why should I choose Immutable.JS as an immutable library?](recipes/usingimmutablejs#why-choose-immutable-js)
- [What are the issues with using Immutable.JS?](recipes/usingimmutablejs#issues-with-immutable-js)
- [Is Immutable.JS worth the effort?](recipes/usingimmutablejs#is-immutable-js-worth-effort)
- [What are some opinionated Best Practices for using Immutable.JS with Redux?](recipes/usingimmutablejs#immutable-js-best-practices)
```

- **Code Structure**

```js
- [What should my file structure look like? How should I group my action creators and reducers in my project? Where should my selectors go?](faq/codestructure#structure-file-structure)
- [How should I split my logic between reducers and action creators? Where should my “business logic” go?](faq/codestructure#structure-business-logic)
```

- Performance
  - Redux 在性能和体系结构方面的“规模”如何？
  - 不会为每个动作调用“所有减速器”会很慢吗？
  - 我是否需要在减速器中克隆我的状态？是不是复制我的状态会变慢？
  - 我如何减少商店更新事件的数量？
  - “拥有一棵树”会导致内存问题吗？将调度许多行动占用内存？
- React Redux
  - 为什么不是我的组件重新渲染，或我的 mapStateToProps 运行？
  - 为什么我的组件经常重新渲染？
  - 我怎样才能加快我的 mapStateToProps ？
  - 为什么我没有在连接组件中使用 this.props.dispatch ？
  - 我应该只连接顶层组件，还是可以连接树中的多个组件？
- Miscellaneous
  - 有没有更大的“真正的” Redux 项目？
  - 我如何在 Redux 中实现身份验证？

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18