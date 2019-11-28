# 使用combineReducers（Using combineReducers）

- 贡献者1人

  

## 核心概念

Redux 应用程序最常见的状态形式是一个简单的Javascript对象，其中包含每个顶级密钥的特定于域的数据的“切片”。同样，编写该状态形状缩减器逻辑的最常用方法是具有“切片缩减器”功能，每个功能都具有相同的`(state, action)`签名，并且每个负责管理对该特定状态片段的所有更新。多切片缩减器可以响应相同的动作，独立地根据需要更新其自己的切片，并且更新的切片被组合到新的状态对象中。

由于这种模式非常普遍，Redux 提供了`combineReducers`实用程序来实现该行为。它是*高阶减速器的*一个例子，它接受一个充满切片减速器功能的对象，并返回一个新的减速器函数。

使用`combineReducers`时有几个重要的想法需要注意：

- 首先，`combineReducers`简单地说，它是**一个实用函数，用于简化编写 Redux Reducer 时最常见的用例**。你是*不是* 需要在自己的应用程序中使用它，它并*不能*处理每一种可能的场景。完全可以在不使用 Reducer 逻辑的情况下编写 Reduce 逻辑，并且对于`combineReducer`不能处理的情况，需要编写自定义Reducer逻辑是很常见的。（请参阅 Beyond `combineReducers`以获取示例和建议。）

- 虽然 Redux 本身并不认为你的状态是如何组织的，但`combineReducers`强制执行若干规则来帮助用户避免常见错误。（详情请参阅`combineReducers`。）

- 一个经常被问到的问题是Redux在调度行动时是否“调用所有减速器”。由于实际上只有一个根减速器功能，所以默认的答案是“不，它不是”。但是，`combineReducers`具有*确实以*这种方式工作的特定行为。为了组装新的状态树，`combineReducers`将调用每个切片缩减器的当前切片状态和当前动作，从而使切片缩减器有机会响应并根据需要更新其状态切片。所以，在这个意义上说，用`combineReducers` *不* “调用所有减速”，或至少所有的切片减速机是包装的。

- 您可以在减速机结构的各个级别使用它，而不仅仅是创建根减速器。在各个地方都有多个组合的减速器是很常见的，它们组合在一起以创建根减速器。

## 定义状态

有两种方法可以定义store状态的初始形状和内容。首先，`createStore`函数可以把`preloadedState`作为第二个参数。这主要用于初始化store，其状态以前在其他地方持续存在，例如浏览器的localStorage。另一种方式是当根参数为时，根还原器返回初始状态值`undefined`。这两种方法在初始化状态中有更详细的描述，但是在使用时还需要注意一些额外的问题`combineReducers`。

`combineReducers`需要一个充满切片减速器功能的对象，并创建一个使用相同键输出相应状态对象的函数。这意味着如果未提供预加载状态`createStore`，则输入片缩减器对象中的键的命名将定义输出状态对象中键的命名。这些名称之间的相关性并不总是很明显，尤其是在使用ES6功能（如默认模块导出和对象字面快捷方式）时。

下面是一个如何使用ES6对象字面速记的例子，`combineReducers`可以定义状态形状：

```javascript
// reducers.js
export default theDefaultReducer = (state = 0, action) => state;

export const firstNamedReducer = (state = 1, action) => state;

export const secondNamedReducer = (state = 2, action) => state;


// rootReducer.js
import {combineReducers, createStore} from "redux";

import theDefaultReducer, {firstNamedReducer, secondNamedReducer} from "./reducers";

// Use ES6 object literal shorthand syntax to define the object shape
const rootReducer = combineReducers({
    theDefaultReducer,
    firstNamedReducer,
    secondNamedReducer
});

const store = createStore(rootReducer);
console.log(store.getState());
// {theDefaultReducer : 0, firstNamedReducer : 1, secondNamedReducer : 2}
```

请注意，因为我们使用 ES6 简写来定义对象字面值，所以生成状态中的键名与导入中的变量名相同。这可能并不总是理想的行为，对于那些不熟悉 ES6 语法的人来说，这往往是一个混乱的原因。

此外，由此产生的名字有点奇怪。在你的状态键名中实际包含诸如 “reducer” 这样的单词通常不是一个好习惯 - 键应该简单地反映它们所持有的域或数据类型。这意味着我们应该显式指定 slice reducer 对象中的键的名称以定义输出状态对象中的键，或者在使用简写对象文字语法时仔细重命名导入切片缩减器的变量以设置键。

更好的用法可能如下所示：

```javascript
import {combineReducers, createStore} from "redux";

// Rename the default import to whatever name we want. We can also rename a named import.
import defaultState, {firstNamedReducer, secondNamedReducer as secondState} from "./reducers";

const rootReducer = combineReducers({
    defaultState,                   // key name same as the carefully renamed default export
    firstState : firstNamedReducer, // specific key name instead of the variable name
    secondState,                    // key name same as the carefully renamed named export
});

const reducerInitializedStore = createStore(rootReducer);
console.log(reducerInitializedStore.getState());
// {defaultState : 0, firstState : 1, secondState : 2}
```

这种状态形状更好地反映了所涉及的数据，因为我们注意设置我们传递给的密钥`combineReducers`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18