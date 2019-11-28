# 初始化状态( Initializing State )

- 贡献者1人

  

有两种主要的方式可以为您的应用程序初始化状态。`createStore`方法可以接受一个`preloadedState`可选值作为其第二个参数。Reducers 还可以通过查找传入状态参数来指定初始值`undefined`，并返回它们想要用作默认值的值。这可以通过在 reducer 中进行显式检查来完成，也可以使用ES6默认参数值语法：`function myReducer(state = someDefaultValue, action)`。

这两种方法如何相互作用并不总是很清楚。幸运的是，这个过程确实遵循了一些可预测的规则。以下是这些部分如何组合在一起。

## 概要

如果没有`combineReducers()`或者类似的手动代码，在reducer中`preloadedState`总是会胜过`state = ...`，因为`state`传递给reducer的*是* `preloadedStatee,`**不是** `undefined`，所以ES6参数语法不适用。

随着`combineReducers()`行为更细微。那些指定了状态的减速器`preloadedState`将会收到该状态。其他reducer将收到`undefined` *，***因为这**将回落到`state = ...`他们指定的默认参数。

一般来说，`preloadedState` 胜过减速器指定的状态。这使得reducer可以指定初始数据，**这些**初始数据*对于他们来说*是默认参数，但也允许在从某些持久性存储或服务器为存储提供水分时加载现有数据（完全或部分）。

注意: 使用*preloadedState* 填充初始状态的减速器仍需要提供一个默认值, 以便在传递 *undefined__* 状态时处理它。所有的减速器在初始化时都未定义, 因此应编写它们, 这样当给定未定义时, 应返回一些值。这可以是任何未定义的值;不需要在此处复制 preloadedState 的部分作为默认值。

## 深入

### 简单减速器

首先让我们考虑一个情况，你有一个 reducer 。假设你不使用`combineReducers()`。

那么你的减速器可能看起来像这样：

```javascript
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT': return state + 1;
  case 'DECREMENT': return state - 1;
  default: return state;
  }
}
```

现在让我们假设你用它创建一个商店。

```javascript
import { createStore } from 'redux';
let store = createStore(counter);
console.log(store.getState()); // 0
```

初始状态为零。 为什么？ 因为 createStore 的第二个参数是未定义的。 这是第一次传递给你的减速器的状态。 Redux 初始化时会分派一个“虚拟”动作来填充状态。 所以你的计数器减速器被调用的状态等于 undefined。 这正是“激活”默认参数的情况。 因此，根据默认状态值（状态= 0），状态现在为0。 这个状态（0）将被返回。

我们来考虑一个不同的场景：

```javascript
import { createStore } from 'redux';
let store = createStore(counter, 42);
console.log(store.getState()); // 42
```

这一次为什么是 42, 而不是0？因为 createStore 被称为42作为第二个参数。这个参数成为状态传递给你的减速机连同假动作。这一次, 状态不是未定义的 (它的 42 ***), 因此 ES6 默认的参数语法没有效果. ** 状态为 42, 42 从减速机返回。



### 组合减速器

现在让我们考虑一下`combineReducers()`使用的情况。

你有两个 reducer：

```javascript
function a(state = 'lol', action) {
  return state;
}

function b(state = 'wat', action) {
  return state;
}
```

`combineReducers({ a, b })`生成的 reducer 看起来像这样：

```javascript
// const combined = combineReducers({ a, b })
function combined(state = {}, action) {
  return {
    a: a(state.a, action),
    b: b(state.b, action)
  };
}
```

如果我们在没有 preloadedState 的情况下调用 createStore 它将初始化状态为 {}。因此, 状态. a 和状态 b 将不确定的时间, 它称为 a 和 b 减速。a 和 b 减速器都将接收未定义的状态参数, 如果它们指定默认状态值, 则将返回这些变量。这就是联合减速器如何在第一次调用时返回 { a: 'lol', b: 'wat' }状态对象。

```javascript
import { createStore } from 'redux';
let store = createStore(combined);
console.log(store.getState()); // { a: 'lol', b: 'wat' }
```

我们来考虑一个不同的场景：

```javascript
import { createStore } from 'redux';
let store = createStore(combined, { a: 'horse' });
console.log(store.getState()); // { a: 'horse', b: 'wat' }
```

现在我指定了`preloadedState`作为`createStore()`的参数。状态返回从组合减速**结合**我指定的初始状态`a`与减速机`'wat'`默认参数指定的`b`减速机选择本身。

让我们回顾一下联合reducer的作用：

```javascript
// const combined = combineReducers({ a, b })
function combined(state = {}, action) {
  return {
    a: a(state.a, action),
    b: b(state.b, action)
  };
}
```

在这种情况下，`state`被指定，所以它没有回落`{}`。这是一个`a`场等于`'horse'`，但没有`b`场的对象。这就是为什么`a`收到的减速`'horse'`为`state`，并返回到存储位置，但`b`收到的减速机`undefined`作为`state`，从而返回**其理念**默认的`state`（在我们的例子中，`'wat'`）。这是我们如何获得`{ a: 'horse', b: 'wat' }`回馈。

## 概括

总之这件事，如果你坚持 Redux 的约定，并返回减速的初始状态时，他们调用`undefined`的`state`参数（来实现，这是指定的最简单的方法`state`ES6默认参数值），你将有组合减速器的一个很好的有用行为。**他们会更喜欢** **传递给函数的对象中的对应值，但是如果您没有传递任何对象，或者未设置相应的字段，则会选择由reducer指定的默认参数。**这种方法运行良好，因为它既提供了现有数据的初始化和水合作用，又允许个别减法器在其数据未保存时重置其状态。当然，你可以递归地应用这个模式，就像你可以使用的一样，`**preloadedState**` `**createStore()**` `**state**` `combineReducers()` 在很多层面上，甚至通过调用reducer并给它们赋予状态树的相关部分来手动组成 reducers。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18