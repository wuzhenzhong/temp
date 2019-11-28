# 先决条件减速器概念( Prerequisite Reducer Concepts )

- 贡献者1人

  

如 Reducers 中所述，Redux reducer 功能：

- 应该有一个签名`(previousState, action) => newState`，类似于你要传递给的函数的类型[`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
- 应该是“纯粹的”，这意味着reducer：
  - 不**执行边缘作用**（例如调用API或修改非本地对象或变量）。
  - 不**调用非纯函数**（如`Date.now`或`Math.random`）。
  - 不**改变它的参数**。如果 reducer 更新状态时，它不应该**修改**了**现有的**就地状态对象。相反，它应该生成一个包含必要更改的**新**对象。对于 reducer 更新状态中的任何子对象，都应使用相同的方法。

> 关于不变性，边缘作用和突变的注意事项不鼓励突变，因为它通常会中断时间行程调试和React Redux的`connect`功能：

- 对于时间旅行，Redux DevTools 期望重放记录的动作会输出一个状态值，但不会改变其他任何东西。**诸如变异或异步行为之类的副作用会导致时间旅行改变步骤之间的行为，从而破坏应用程序**。
- 对于 React Redux，`connect`检查是否`mapStateToProps`已更改从函数返回的道具以确定组件是否需要更新。为了提高性能，`connect`需要一些依赖状态不可变的快捷方式，并使用浅层引用相等检查来检测更改。这意味着**不会检测到通过直接变异对对象和数组所做的更改，并且组件不会重新呈现**。

其他副作用，例如在 reducer 中生成唯一ID或时间戳，也会使代码难以预测并且难以调试和测试。

由于这些规则，在转向组织 Redux reducer 的其他特定技术之前，必须充分理解以下核心概念：

#### Redux Reducer基础

**重要概念**：

- 从状态和状态形状的角度思考
- 由状态委托更新责任（**减速器组成**）
- 更高阶的减速器
- 定义减速器的初始状态

**阅读清单**：

- Redux Docs: Reducers
- Redux Docs: Reducing Boilerplate
- Redux Docs: Implementing Undo History
- Redux Docs: `combineReducers`
- [The Power of Higher-Order Reducers](http://slides.com/omnidan/hor#/)
- [Stack Overflow: Store initial state and `combineReducers`](http://stackoverflow.com/questions/33749759/read-stores-initial-state-in-redux-reducer)
- [Stack Overflow: State key names and `combineReducers`](http://stackoverflow.com/questions/35667775/state-in-redux-react-app-has-a-property-with-the-name-of-the-reducer)

#### 纯粹的功能和副作用

**重要概念**：

- 副作用
- 纯粹的功能
- 如何结合功能来思考

**阅读清单**：

- [The Little Idea of Functional Programming](http://jaysoo.ca/2016/01/13/functional-programming-little-ideas/)
- [Understanding Programmatic Side-Effects](http://c2fo.io/c2fo/programming/2016/05/11/understanding-programmatic-side-effects/)
- [Learning Functional Programming in Javascript](https://youtu.be/e-5obm1G_FY)
- [An Introduction to Reasonably Pure Functional Programming](https://www.sitepoint.com/an-introduction-to-reasonably-pure-functional-programming/)

#### 不可变的数据管理

**重要概念**：

- 易变性与不变性
- 不间断地更新对象和数组
- 避免使状态发生变化的函数和语句

**阅读清单**：

- [Pros and Cons of Using Immutability With React](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)
- [Javascript and Immutability](http://t4d.io/javascript-and-immutability/)
- [Immutable Data using ES6 and Beyond](http://wecodetheweb.com/2016/02/12/immutable-javascript-using-es6-and-beyond/)
- [Immutable Data from Scratch](https://ryanfunduk.com/articles/immutable-data-from-scratch/)
- Redux Docs: Using the Object Spread Operator

#### 规范化数据

**重要概念**：

- 数据库结构和组织
- 将关系/嵌套数据拆分成单独的表格
- 为给定项目存储单个定义
- 通过ID引用项目
- 使用按物品ID键入的对象作为查找表，以及用于跟踪排序的ID数组
- 关联关系中的项目

**阅读清单**：

- [Database Normalization in Simple English](http://www.essentialsql.com/get-ready-to-learn-sql-database-normalization-explained-in-simple-english/)
- [Idiomatic Redux: Normalizing the State Shape](https://egghead.io/lessons/javascript-redux-normalizing-the-state-shape)
- [Normalizr Documentation](https://github.com/paularmstrong/normalizr)
- [Redux Without Profanity: Normalizr](https://tonyhb.gitbooks.io/redux-without-profanity/content/normalizer.html)
- [Querying a Redux Store](https://medium.com/@adamrackis/querying-a-redux-store-37db8c7f3b0f)
- [Wikipedia: Associative Entity](https://en.wikipedia.org/wiki/Associative_entity)
- [Database Design: Many-to-Many](http://www.tomjewett.com/dbdesign/dbdesign.php?page=manymany.php)
- [Avoiding Accidental Complexity When Structuring Your App State](https://medium.com/@talkol/avoiding-accidental-complexity-when-structuring-your-app-state-6e6d22ad5e2a)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18