# 拆分Reducer逻辑（Splitting Up Reducer Logic）

- 贡献者1人

  

对于任何有意义的应用程序，将*所有*更新逻辑放入单个 reducer 函数中很快就会变得无法维护。虽然函数应该存在多长时间没有单独的规则，但通常认为函数应该相对较短，理想情况下只能做一件特定的事情。正因为如此，编写很长或者做很多不同事情的代码片段是很好的编程习惯，并且将它们分解成更易于理解的小块。

由于Redux reducer *只是*一个函数，所以适用相同的概念。你可以将你的reducer逻辑分成另一个函数，并从父函数中调用这个新函数。

这些新函数通常可以分为三类：

1. 小型实用程序函数包含多个地方需要的一些可重用逻辑块（实际上可能与实际业务逻辑相关，也可能不实际相关）

\2. 用于处理特定的更新的情况下的函数，这通常需要比典型的其它参数`(state, action)`对

\3. 处理给定切片状态的*所有*更新的函数。这些功能通常具有典型的`(state, action)`参数签名

为了清楚起见，这些术语将用于区分不同类型的函数和不同的使用情况：

- ***reducer***：与签名的任何函数`(state, action) -> newState`（即，任何函数*可*被用作参数`Array.prototype.reduce`）

- ***root reducer***：实际作为第一个参数传递给的reducer函数`createStore`。这是*必须*具有`(state, action) -> newState`签名的还原器逻辑的唯一部分。

- ***slice reducer***：一种正在用于处理状态树的特定片断更新的简化器，通常通过传递给它来完成`combineReducers`

- ***case函数***：用于处理特定操作的更新逻辑的函数。这实际上可能是一个减速器函数，或者可能需要其他参数才能正常工作。

- ***高阶reducer***：一个将简化器函数作为参数的函数，并且/或者返回一个新的简化器函数作为结果（比如`combineReducers`或`redux-undo`）

“ *sub-reducer*” 这个术语也被用于各种讨论中，意思是任何不是root reducer的函数，尽管这个术语不是很精确。有些人还可能将某些功能称为“ *业务逻辑* ”（与应用程序特定行为相关的功能）或“ *实用功能* ”（不是特定于应用程序的通用功能）。

将复杂的过程分解为更小，更容易理解的部分通常用术语 [***functional decomposition*** ](http://stackoverflow.com/questions/947874/what-is-functional-decomposition)来描述。这个术语和概念可以泛泛地应用于任何代码。但是，在 Redux 中，使用方法＃3构造 reducer 逻辑*非常*普遍，其中更新逻辑根据状态片委托给其他函数。Redux 将此概念称为 ***reducer composition***，它是迄今为止构建 reducer 逻辑最广泛使用的方法。实际上， Redux 包含一个名为 “utility” 的实用程序是很常见的`combineReducers()`，它特别提取了将工作委托给其他基于状态切片的reducer函数的过程。但是，重要的是要注意它不是*唯一的*可以使用的图案。事实上，使用所有三种方法将逻辑分解为函数是完全可能的，通常也是一个好主意。Refactoring Reducers 部分显示了这个实例的一些例子。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18