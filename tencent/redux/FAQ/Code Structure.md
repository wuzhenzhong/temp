# Redux FAQ：代码结构（Code Structure）

- 贡献者2人

  

## 目录

- 我的文件结构应该是什么样子？我应该如何将我的动作创建者和缩减者分组到我的项目中？我的选择器应该到哪里去？

- 我应该如何在减速器和动作创造者之间分配我的逻辑？我的“业务逻辑”应该放在哪里？

## 代码结构

### 我的文件结构应该是什么样子？我应该如何将我的动作创建者和缩减者分组到我的项目中？我的选择器应该到哪里去？

由于 Redux 只是一个数据存储库，因此对于如何构建项目没有直接的意见。但是，大多数 Redux 开发人员倾向于使用一些常见模式：

- Rails 风格：“actions”，“constants”，“redurs”，“containers”和“components”的单独文件夹

- 域样式：每个功能或域的单独文件夹，可能包含每个文件类型的子文件夹

- “Ducks”：与域风格类似，但通常通过将它们定义在同一个文件中来明确地将动作和缩减器绑定在一起

通常建议选择器与 reducer 一起定义并导出，然后在别处重新使用（例如在`mapStateToProps`函数中，在异步动作创建者或sagas中）以合并所有知道 reducer 文件中状态树实际形状的代码。

虽然最终无关紧要地将代码放在磁盘上，但重要的是要记住，不应该孤立地考虑动作和缩减器。完全可能（并鼓励）在一个文件夹中定义的 reducer 响应另一个文件夹中定义的操作。

#### 更多信息

**文档**

- FAQ: Actions - "1:1 mapping between reducers and actions?"（操作 - “减压器和操作之间的1：1映射？”）

### 文章

- [How to Scale React Applications](https://www.smashingmagazine.com/2016/09/how-to-scale-react-applications/) (accompanying talk: [Scaling React Applications](https://vimeo.com/168648012)) （如何扩展React应用程序（随附讨论：缩放React应用程序））

- [Redux Best Practices（](https://medium.com/lexical-labs-engineering/redux-best-practices-64d59775802e)Redux最佳实践[）](https://medium.com/lexical-labs-engineering/redux-best-practices-64d59775802e)

- [Rules For Structuring (Redux) Applications （](http://jaysoo.ca/2016/02/28/organizing-redux-application/)规则构建（Redux）应用程序[）](http://jaysoo.ca/2016/02/28/organizing-redux-application/)

- [A Better File Structure for React/Redux Applications（](http://marmelab.com/blog/2015/12/17/react-directory-structure.html)React/Redux应用程序的更好的文件结构[）](http://marmelab.com/blog/2015/12/17/react-directory-structure.html)

- [Organizing Large React Applications（](http://engineering.kapost.com/2016/01/organizing-large-react-applications/)组织大的反应应用程序[）](http://engineering.kapost.com/2016/01/organizing-large-react-applications/)

- [Four Strategies for Organizing Code（](https://medium.com/@msandin/strategies-for-organizing-code-2c9d690b6f33)组织代码的四种策略[）](https://medium.com/@msandin/strategies-for-organizing-code-2c9d690b6f33)

- [Encapsulating the Redux State Tree（](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)封装Redux状态树[）](http://randycoulman.com/blog/2016/09/13/encapsulating-the-redux-state-tree/)

- [Redux Reducer/Selector Asymmetry（](http://randycoulman.com/blog/2016/09/20/redux-reducer-selector-asymmetry/)Redux减速器/选择器不对称[）](http://randycoulman.com/blog/2016/09/20/redux-reducer-selector-asymmetry/)

- [Modular Reducers and Selectors（](http://randycoulman.com/blog/2016/09/27/modular-reducers-and-selectors/)模块化减速器和选择器[）](http://randycoulman.com/blog/2016/09/27/modular-reducers-and-selectors/)

- [My journey towards a maintainable project structure for React/Redux（](https://medium.com/@mmazzarolo/my-journey-toward-a-maintainable-project-structure-for-react-redux-b05dfd999b5)我的React/Redux可维护项目结构[）](https://medium.com/@mmazzarolo/my-journey-toward-a-maintainable-project-structure-for-react-redux-b05dfd999b5)

- [React/Redux Links: Architecture - Project File Structure（](https://github.com/markerikson/react-redux-links/blob/master/react-redux-architecture.md#project-file-structure)React/Redux链接：体系结构 - 项目文件结构[）](https://github.com/markerikson/react-redux-links/blob/master/react-redux-architecture.md#project-file-structure)

**讨论**

- [#839: Emphasize defining selectors alongside reducers（](https://github.com/reactjs/redux/issues/839)强调与缩减者一起定义选择器[）](https://github.com/reactjs/redux/issues/839)

- [#943: Reducer querying（](https://github.com/reactjs/redux/issues/943)Reducer查询[）](https://github.com/reactjs/redux/issues/943)

- [React Boilerplate #27: Application Structure（](https://github.com/mxstbr/react-boilerplate/issues/27)React Boilerplate＃27：应用程序结构[）](https://github.com/mxstbr/react-boilerplate/issues/27)

- [Stack Overflow: How to structure Redux components/containers（](http://stackoverflow.com/questions/32634320/how-to-structure-redux-components-containers/32921576)堆栈溢出：如何构建Redux组件/容器[）](http://stackoverflow.com/questions/32634320/how-to-structure-redux-components-containers/32921576)

- [Twitter: There is no ultimate file structure for Redux（](https://twitter.com/dan_abramov/status/783428282666614784)Twitter：Redux没有最终的文件结构[）](https://twitter.com/dan_abramov/status/783428282666614784)

### 我应该如何在减速器和动作创造者之间分配我的逻辑？我的“业务逻辑”应该放在哪里？

关于在缩减器或动作创建器中究竟应该采用哪些逻辑，没有单一的明确答案。一些开发人员更喜欢拥有“胖”动作创作者，而“瘦”减速器只是将动作中的数据取而代之，并盲目地将其合并到相应的状态中。其他人则试图强调保持尽可能小的行为，并尽量减少`getState()`动作创建者的使用。（为了这个问题的目的，其他异步方法，如传奇和守望者属于“动作创造者”类别。）

这个评论很好地总结了二分法：

> 现在，问题在于动作创建者以及减速器中的内容，即胖动作对象和瘦动作对象之间的选择。如果将所有逻辑放入动作创建器中，则最终会得到基本上声明状态更新的胖动作对象。减速器变得纯粹，笨拙，添加 - 删除它，更新这些功能。他们将很容易撰写。但是你的业务逻辑不会太多。如果你在 reducer 中加入更多的逻辑，你最终会得到很好的精简动作对象，大部分的数据逻辑集中在一个地方，但是你的 reducer 很难编写，因为你可能需要来自其他分支的信息。你最终会得到大型减速器或减速器，这些减速器或减速器会从该州的较高位置获得额外的参数。

找到这两个极端之间的平衡点，你将掌握 Redux。

#### 更多信息

文章

- [Where do I put my business logic in a React/Redux application?（](https://medium.com/@jeffbski/where-do-i-put-my-business-logic-in-a-react-redux-application-9253ef91ce1)我在哪里把我的商业逻辑放在React/Redux应用程序中？[）](https://medium.com/@jeffbski/where-do-i-put-my-business-logic-in-a-react-redux-application-9253ef91ce1)

- [How to Scale React Applications（](https://www.smashingmagazine.com/2016/09/how-to-scale-react-applications/)如何缩放React应用程序[）](https://www.smashingmagazine.com/2016/09/how-to-scale-react-applications/)

- [The Tao of Redux, Part 2 - Practice and Philosophy. Thick and thin reducers.（Redux的道，Part 2 -实践和哲学，厚和薄的reducers）](http://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/#thick-and-thin-reducers)

**讨论**

- [How putting too much logic in action creators could affect debugging （](https://github.com/reactjs/redux/issues/384#issuecomment-127393209)如何在动作创建者中放置太多逻辑可能会影响调试[）](https://github.com/reactjs/redux/issues/384#issuecomment-127393209)

- [#1165: Where to put business logic / validation? （](https://github.com/reactjs/redux/issues/1165)在哪里放置业务逻辑/验证？[）](https://github.com/reactjs/redux/issues/1165)

- [#1171: Recommendations for best practices regarding action-creators, reducers, and selectors（](https://github.com/reactjs/redux/issues/1171)针对行动创建者，减速器和选择者的最佳实践建议[）](https://github.com/reactjs/redux/issues/1171)

- [Stack Overflow: Accessing Redux state in an action creator?（](http://stackoverflow.com/questions/35667249/accessing-redux-state-in-an-action-creator/35674575)堆栈溢出：在动作创建器中访问Redux状态？[）](http://stackoverflow.com/questions/35667249/accessing-redux-state-in-an-action-creator/35674575)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18