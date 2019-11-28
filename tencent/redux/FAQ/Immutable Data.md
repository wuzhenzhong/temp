# Redux FAQ：不可变数据（Immutable Data）

- 贡献者2人

  

## 目录

- 不变性有什么好处？
- Redux 为什么要求不变性？
- 为什么 Redux 使用浅层平等检查需要不变性？
  - 浅层和深层平等检查有何不同？
  - Redux 如何使用浅层平等检查？
  - 如何`combineReducers`使用浅层平等检查？
  - React-Redux 如何使用浅层平等检查？
  - React-Redux 如何使用浅层平等检查来确定组件是否需要重新渲染？
  - 为什么浅层平等检查不适用于可变对象？
  - 使用可变对象进行浅平等检查是否会导致 Redux 出现问题？
  - 为什么 reducer 改变状态会阻止 React-Redux 重新渲染一个被包装的组件？
  - 为什么选择器会突变并返回一个持久对象，以`mapStateToProps`防止 React-Redux 重新渲染一个被包装的组件？
  - 不变性如何启用浅层检查来检测对象突变？
- 减速器中的不变性如何导致组件不必要地渲染？
- mapStateToProps 中的不变性如何导致组件不必要地渲染？
- 有什么办法可以永久处理数据？我必须使用 Immutable.JS 吗？
- 使用JavaScript 进行不可变操作有什么问题？

## 不变性有什么好处？

不变性可以为您的应用带来更高的性能，并导致更简单的编程和调试，因为永远不会改变的数据比在您的应用中随意更改的数据更容易推理。

特别是，Web 应用程序环境中的不变性使复杂的变更检测技术能够简单而廉价地实施，从而确保更新 DOM 的计算成本较高的过程只有在绝对必须时才会发生（React 性能改进的基石超过其他库） 。

#### 更多信息

文章

- [Introduction to Immutable.js and Functional Programming Concepts（](https://auth0.com/blog/intro-to-immutable-js/)Immutable.js和函数式编程概念介绍[）](https://auth0.com/blog/intro-to-immutable-js/)
- [JavaScript Immutability presentation (PDF - see slide 12 for benefits)（](https://www.jfokus.se/jfokus16/preso/JavaScript-Immutability--Dont-Go-Changing.pdf)JavaScript不变性演示文稿（PDF - 请参见幻灯片12了解优点）[）](https://www.jfokus.se/jfokus16/preso/JavaScript-Immutability--Dont-Go-Changing.pdf)
- [Immutable.js - Immutable Collections for JavaScript（](https://facebook.github.io/immutable-js/#the-case-for-immutability)Immutable.js - 用于JavaScript的不可变集合[）](https://facebook.github.io/immutable-js/#the-case-for-immutability)
- [React: Optimizing Performance（](https://facebook.github.io/react/docs/optimizing-performance.html)反应：优化性能[）](https://facebook.github.io/react/docs/optimizing-performance.html)
- [JavaScript Application Architecture On The Road To 2015（](https://medium.com/google-developers/javascript-application-architecture-on-the-road-to-2015-d8125811101b#.djje0rfys)在到2015年的路上的JavaScript应用程序体系结构[）](https://medium.com/google-developers/javascript-application-architecture-on-the-road-to-2015-d8125811101b#.djje0rfys)

## Redux 为什么要求不变性？

- Redux 和 React-Redux 都采用浅层平等检查。尤其是：
  - Redux的`combineReducers`实用程序浅显地检查由它调用的reducer引起的引用更改。
  - React-Redux的`connect`方法生成的组件会浅显地检查对根状态的引用更改，并从`mapStateToProps`函数返回值以查看被包装的组件是否实际需要重新呈现。这种浅层检查需要不变性才能正常工作。
- 不变的数据管理最终使数据处理更安全。
- 时间旅行调试要求减速器是纯粹的函数，无副作用，以便您可以在不同状态之间正确跳转。

#### 更多信息

**文档**

- Recipes: 先决条件减速器的概念

**讨论**

- [Reddit: Why Redux Needs Reducers To Be Pure Functions](https://www.reddit.com/r/reactjs/comments/5ecqqv/why_redux_need_reducers_to_be_pure_functions/dacmmjh/?context=3)（Reddit：为什么Redux需要减速器才是纯功能）

## 为什么 Redux 使用浅层平等检查需要不变性？

如果要正确更新任何连接的组件，Redux 使用浅层平等检查就需要不变性。 为了明白为什么，我们需要了解 JavaScript 中浅层和深层平等检查的区别。

### 浅层和深层平等检查有何不同？

浅平等检查（或*引用相等*）只是检查两个不同的*变量*引用同一个对象; 相反，深度平等检查（或*值相等*）必须检查两个对象属性的每个*值*。

浅平等检查因此是简单的（和快速）`a === b`，而深度平等检查涉及通过两个对象的属性进行递归遍历，比较每个步骤中每个属性的值。

Redux 使用浅层平等检查来改善性能。

#### 更多信息

**文章**

- [Pros and Cons of using immutability with React.js（](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)React.js使用不变性的优点和缺点[）](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

### Redux 如何使用浅层平等检查？

Redux 在其 `combineReducers`函数中使用浅层平等检查来返回根状态对象的新变异副本，或者，如果没有进行突变，则返回当前根状态对象。

#### 更多信息

**文档**

- API: combineReducers

#### `combineReducers`如何使用浅层平等检查？

Redux 商店的建议结构是通过密钥将状态对象分割为多个“切片”或“域”，并提供单独的缩减器功能来管理每个单独的数据切片。

`combineReducers`通过将`reducers`参数定义为包含一组键/值对的哈希表（其中每个键是状态片的名称）以及相应的值是将作用于它。

所以，例如，如果你的状态是`{ todos, counter }`，那么调用`combineReducers`将是：

```javascript
combineReducers({ todos: myTodosReducer, counter: myCounterReducer })
```

其中：

- 这些密钥`todos`和`counter`每一个引用一个单独的状态片;
- 值`myTodosReducer`和`myCounterReducer`是减速功能，每个作用在由相应的键标识的状态的片。

`combineReducers`遍历每个键/值对。对于每一次迭代，它：

- 创建对由每个键引用的当前状态片段的引用；
- 调用相应的减速器并将其传递给切片；
- 创建一个对由 reducer 返回的可能发生变化的状态切片的引用。

随着迭代的继续，`combineReducers`将使用从每个reducer返回的状态片构造一个新的状态对象。这个新的状态对象可能与当前状态对象不同，也可能不同。它在这里`combineReducers`使用浅层平等检查来确定状态是否已经改变。

具体而言，在迭代的每个阶段，`combineReducers`对当前状态片和从还原器返回的状态片执行浅层次的相等检查。如果 reducer 返回一个新对象，浅层相等性检查将失败，并将标志`combineReducers`设置`hasChanged`为 true。

迭代完成后，`combineReducers`将检查`hasChanged`标志的状态。如果是，则返回新构造的状态对象。如果它为假，则返回*当前*状态对象。

这值得强调：*如果 reducer 都返回* *传递给它们的同一个对象，那么将返回当前的根状态对象，而不是新更新的对象。state* *combineReducers*

#### 更多信息

**文档**

- API: combineReducers
- Redux FAQ - 我如何在两个减速器之间共享状态？ 我必须使用`combineReducers`吗？

**视频**

- [Egghead.io: Redux: Implementing combineReducers() from Scratch（](https://egghead.io/lessons/javascript-redux-implementing-combinereducers-from-scratch)Egghead.io：Redux：从Scratch实现combineReducers()[）](https://egghead.io/lessons/javascript-redux-implementing-combinereducers-from-scratch)

### React-Redux 如何使用浅层平等检查？

React-Redux 使用浅层次的相等检查来确定它包装的组件是否需要重新渲染。

为此，它假定被包装的组件是纯的; 也就是说，[考虑到相同的道具和状态](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)，该组件将产生[相同的结果](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)。

通过假设包装组件是纯的，它只需检查根状态对象或返回的值是否`mapStateToProps`已更改。如果他们没有，则被包装的组件不需要重新渲染。

它通过保持对根状态对象的引用以及对从`mapStateToPro`函数返回的 props 对象中*每个值*的引用来检测更改。

然后，它对其对根状态对象和传递给它的状态对象的引用进行浅层次的等式检查，并且对每个对道具对象值的引用以及从`mapStateToProps`再次运行该函数所返回的引用进行一系列浅层检查。

#### 更多信息

**文档**

- React-Redux Bindings

**文章**

- [API: React-Redux’s connect function and `mapStateToProps（`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)API：React-Redux的连接函数和mapStateToProps[`）`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)
- [Troubleshooting: My views aren’t updating when something changes outside of Redux（](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)故障排除：当Redux外部发生变化时，我的视图不会更新[）](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)

### 为什么React-Redux会浅显地检查从`mapStateToProp`对象返回的props对象中的每个值？

React-Redux 对道具对象中的每个*值*执行浅层平均检查，而不是对道具对象本身。

它是这样做的，因为 props 对象实际上是 prop 名称及其值（或用于检索或生成值的选择器函数）的哈希，如下例所示：

```javascript
function mapStateToProps(state) {
  return {
    todos: state.todos, // prop value
    visibleTodos: getVisibleTodos(state) // selector
  }
}

export default connect(mapStateToProps)(TodoApp)
```

因此，从重复调用返回的道具对象的浅层相等检查`mapStateToProps`总是会失败，因为每次都会返回一个新对象。

React-Redux 因此在返回的道具对象中维护对每个*值的*单独引用。

#### 更多信息

**文章**

- [React.js pure render performance anti-pattern（](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.gh07cm24f)React.js纯粹渲染性能反模式[）](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.gh07cm24f)

### React-Redux 如何使用浅层平等检查来确定组件是否需要重新渲染？

每次`connect`调用React-Redux 函数时，它都会对其存储的对根状态对象的引用以及从存储区传递给它的当前根状态对象执行浅层次的相等检查。如果检查通过，则根状态对象尚未更新，因此不需要重新呈现组件，甚至不需要调用`mapStateToProps`。

如果检查失败，但是，根状态对象*已*被更新，因此`connect`将调用`mapStateToProps`，查看是否有包装的组件道具已被更新。

它通过对对象内的每个值分别执行浅的相等检查来执行此操作，并且只有在其中一个检查失败时才会触发重新呈现。

在下面的示例中，如果`state.todos`和返回的值`getVisibleTodos()`在连续调用时不会更改`connect`，则组件不会重新呈现。

```javascript
function mapStateToProps(state) {
  return {
    todos: state.todos, // prop value
    visibleTodos: getVisibleTodos(state) // selector
  }
}

export default connect(mapStateToProps)(TodoApp)
```

相反，在下一个示例中（下面），组件将*始终*重新呈现，因为值`todos`始终是新对象，无论其值是否更改：

```javascript
// AVOID - will always cause a re-render
function mapStateToProps(state) {
  return {
    // todos always references a newly-created object
    todos: {
      all: state.todos,
      visibleTodos: getVisibleTodos(state)
    }
  }
}

export default connect(mapStateToProps)(TodoApp)
```

如果在从`mapStateToProps`之前返回的新值和 React-Redux 保留引用的先前值之间，浅层相等检查失败，则将触发组件的重新呈现。

#### 更多信息

**文章**

- [Practical Redux, Part 6: Connected Lists, Forms, and Performance（](http://blog.isquaredsoftware.com/2017/01/practical-redux-part-6-connected-lists-forms-and-performance/)Practical Redux，第6部分：连接列表，表单和性能[）](http://blog.isquaredsoftware.com/2017/01/practical-redux-part-6-connected-lists-forms-and-performance/)
- [React.js Pure Render Performance Anti-Pattern（](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.sb708slq6)React.js纯渲染性能反模式[）](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.sb708slq6)
- [High Performance Redux Apps（](http://somebody32.github.io/high-performance-redux/)高性能Redux应用程序[）](http://somebody32.github.io/high-performance-redux/)

**讨论**

- [#1816: Component connected to state with `mapStateToProps`（](https://github.com/reactjs/redux/issues/1816)通过mapStateToProps连接到状态的组件[）](https://github.com/reactjs/redux/issues/1816)
- [#300: Potential connect() optimization（](https://github.com/reactjs/react-redux/issues/300)潜在的connect()优化）

### 为什么浅层平等检查不适用于可变对象？

如果该对象是可变的，则浅平等检查不能用于检测函数是否改变了传入它的对象。

这是因为引用相同对象的两个变量*总是*相等的，无论对象的值是否更改，因为它们都引用同一个对象。因此，以下将始终返回 true：

```javascript
function mutateObj(obj) {
  obj.key = 'newValue'
  return obj
}

const param = { key: 'originalValue' }
const returnVal = mutateObj(param)

param === returnVal
//> true
```

浅层检查`param`和`returnValue`简单检查两个变量是否引用同一个对象，他们都这样做。`mutateObj()`可能会返回一个突变版本`obj`，但它仍然是传入的同一个对象。事实上，它的值在内部`mutateObj`事件中已经发生变化，而不是简单的检查。

#### 更多信息

**文章**

- [Pros and Cons of using immutability with React.js（](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)React.js使用不变性的优点和缺点[）](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

### 使用可变对象进行浅平等检查是否会导致 Redux 出现问题？

使用可变对象进行浅平等检查不会导致Redux出现问题，但会导致依赖于存储的库（如React-Redux）出现问题。

具体来说，如果传递给 reducer 的状态片`combineReducers`是可变对象，reducer 可以直接修改它并返回它。

如果是这样，执行的浅层相等性检查`combineReducers`将始终通过，因为 reducer 返回的状态片的值可能已经发生了变化，但对象本身没有 - 它仍然是传递给reducer的同一个对象。

因此，即使国家已经改变，`combineReducers`也不会设置`hasChanged`旗帜。如果其他 reducer 中没有一个返回新的更新状态片，则该`hasChanged`标志将保持设置为 false，从而`combineReducers`返回*现有的*根状态对象。

商店仍然会更新为根状态的新值，但由于根状态对象本身仍然是同一个对象，因此绑定到 Redux 的库（例如React-Redux）将不会意识到状态的变化，并且所以不会触发包装组件的重新渲染。

#### 更多信息

**文档**

- 菜单：不变的更新模式
- 故障排除：不要改变减速器参数

### 为什么 reducer 改变状态会阻止 React-Redux 重新渲染一个被包装的组件？

如果一个 Redux reducer 直接对状态对象进行变异并返回，那么根状态对象的值将会改变，但对象本身不会。

由于 React-Redux 对根状态对象执行浅层检查以确定其包装组件是否需要重新呈现，因此它将无法检测到状态变化，因此不会触发重新呈现。

#### 更多信息

**文档**

- [Troubleshooting: My views aren’t updating when something changes outside of Redux（](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)故障排除：当Redux外部发生变化时，我的视图不会更新[）](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)

### 为什么选择器会突变并返回一个持久对象，以`mapStateToProps`防止 React-Redux 重新渲染一个被包装的组件？

如果从`mapStateToPro`对象返回的 props 对象的其中一个值是一个持续跨越调用的对象`connect`（例如潜在的根状态对象），但是直接进行了变异并由选择器函数返回，则 React-Redux 将无法检测突变，因此不会触发重新渲染封装组件。

正如我们所看到的，选择器函数返回的可变对象中的值可能已经改变，但是对象本身没有，并且浅的相等性检查只比较对象本身，而不是它们的值。

例如，以下`mapStateToProps`函数永远不会触发重新渲染：

```javascript
// State object held in the Redux store
const state = {
  user: {
    accessCount: 0,
    name: 'keith'
  }
}

// Selector function
const getUser = state => {
  ++state.user.accessCount // mutate the state object
  return state
}

// mapStateToProps
const mapStateToProps = state => ({
  // The object returned from getUser() is always
  // the same object, so this wrapped
  // component will never re-render, even though it's been
  // mutated
  userRecord: getUser(state)
})

const a = mapStateToProps(state)
const b = mapStateToProps(state)

a.userRecord === b.userRecord
//> true
```

请注意，相反，如果使用*不可变*对象，则在不应该组件时，组件可能会重新渲染。

#### 更多信息

**文章**

- [Practical Redux, Part 6: Connected Lists, Forms, and Performance（](http://blog.isquaredsoftware.com/2017/01/practical-redux-part-6-connected-lists-forms-and-performance/)Practical Redux，第6部分：连接列表，表单和性能[）](http://blog.isquaredsoftware.com/2017/01/practical-redux-part-6-connected-lists-forms-and-performance/)

**讨论**

- [#1948: Is getMappedItems an anti-pattern in mapStateToProps?（](https://github.com/reactjs/redux/issues/1948)getMappedItems是mapStateToProps中的反模式吗？[）](https://github.com/reactjs/redux/issues/1948)

### 不变性如何启用浅层检查来检测对象突变？

如果一个对象是不可变的，则必须对该对象的*副本*进行任何需要对其进行的更改。

这个变异副本是一个*与*传入函数的对象*不同的单独*对象，所以当它返回时，浅层检查会将它识别为与传入的对象不同的对象，因此将失败。

#### 更多信息

**文章**

- [Pros and Cons of using immutability with React.js（](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)React.js使用不变性的优点和缺点[）](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

### 减速器中的不变性如何导致组件不必要地渲染？

你不能改变一个不可变的对象; 相反，你必须改变它的一个副本，保持原来的完整。

当你修改副本时，这是完全正确的，但是在 reducer 的上下文中，如果你返回一个*没有*变异的副本，Redux 的`combineReducers`函数仍然会认为状态需要更新，因为你返回的是完全不同的对象来自传入的状态切片对象。

`combineReducers`然后将这个新的根状态对象返回给商店。新对象将具有与当前根状态对象相同的值，但由于它是不同的对象，因此会导致更新存储，这将最终导致所有连接的组件不必要地重新呈现。

为了防止这种情况发生，*如果 reducer 不改变状态*，则必须*始终返回传递给 reducer 的状态切片对象。*

#### 更多信息

**文章**

- [React.js pure render performance anti-pattern（](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.5hmnwygsy)React.js纯粹渲染性能反模式[）](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.5hmnwygsy)
- [Building Efficient UI with React and Redux（](https://www.toptal.com/react/react-redux-and-immutablejs)使用React和Redux构建高效的用户界面[）](https://www.toptal.com/react/react-redux-and-immutablejs)

### `mapStateToProp`中的不可变性如何导致组件不必要的渲染？

某些不可变操作（如数组过滤器）将始终返回一个新对象，即使这些值本身没有更改。

如果将这样的操作用作选择器函数`mapStateToProps`，则 React-Redux 对返回的 props 对象中的每个值执行的浅层相等检查将始终失败，因为每次选择器都返回一个新对象。

因此，即使这个新对象的值没有改变，被包装的组件总是会被重新渲染，

例如，以下将始终触发重新渲染：

```javascript
// A JavaScript array's 'filter' method treats the array as immutable,
// and returns a filtered copy of the array.
const getVisibleTodos = todos => todos.filter(t => !t.completed)

const state = {
  todos: [
    {
      text: 'do todo 1',
      completed: false
    },
    {
      text: 'do todo 2',
      completed: true
    }
  ]
}

const mapStateToProps = state => ({
  // getVisibleTodos() always returns a new array, and so the
  // 'visibleToDos' prop will always reference a different array,
  // causing the wrapped component to re-render, even if the array's
  // values haven't changed
  visibleToDos: getVisibleTodos(state.todos)
})

const a = mapStateToProps(state)
//  Call mapStateToProps(state) again with exactly the same arguments
const b = mapStateToProps(state)

a.visibleToDos
//> { "completed": false, "text": "do todo 1" }

b.visibleToDos
//> { "completed": false, "text": "do todo 1" }

a.visibleToDos === b.visibleToDos
//> false
```

请注意，相反，如果您的 props 对象中的值引用可变对象，那么您的组件在它应该显示时可能不会呈现。

#### 更多信息

**文章**

- [React.js pure render performance anti-pattern（](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.b8bpx1ncj)React.js纯粹渲染性能反模式[）](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.b8bpx1ncj)
- [Building Efficient UI with React and Redux（](https://www.toptal.com/react/react-redux-and-immutablejs)使用React和Redux构建高效的用户界面[）](https://www.toptal.com/react/react-redux-and-immutablejs)
- [ImmutableJS: worth the price?（](https://medium.com/@AlexFaunt/immutablejs-worth-the-price-66391b8742d4#.a3alci2g8)ImmutableJS：值得的价格？[）](https://medium.com/@AlexFaunt/immutablejs-worth-the-price-66391b8742d4#.a3alci2g8)

## 有什么办法可以永久处理数据？我必须使用 Immutable.JS 吗？

Redux 不需要使用 Immutable.JS 。普通的 JavaScript ，如果编写正确，完全可以提供不变性，而不必使用不可变的库。

但是，使用 JavaScript 保证不变性是很困难的，并且可能很容易使对象发生意外变异，从而导致应用程序中很难找到的错误。因此，使用不可变更新实用程序库（如 Immutable.JS）可以显着提高应用程序的可靠性，并使应用程序的开发更加轻松。

#### 更多信息

**讨论**

- [#1185: Question: Should I use immutable data structures?（](https://github.com/reactjs/redux/issues/1422)问题：我应该使用不可变数据结构吗？[）](https://github.com/reactjs/redux/issues/1422)
- [Introduction to Immutable.js and Functional Programming Concepts（](https://auth0.com/blog/intro-to-immutable-js/)Immutable.js和函数式编程概念介绍[）](https://auth0.com/blog/intro-to-immutable-js/)

## 使用普通 JavaScript 进行不可变操作有什么问题？

JavaScript 从未被设计为提供有保证的不可变操作。因此，如果您选择将其用于 Redux 应用程序中的不可变操作，则需要注意以下几个问题。

### 意外物体突变

使用 JavaScript，您可以在不意识到的情况下轻易地改变对象（例如 Redux状 态树）。例如，更新深层嵌套属性，创建对象而不是新对象的新*引用*，或者执行浅拷贝而不是深拷贝，都可能导致无意中的对象突变，甚至可能导致经验最丰富的 JavaScript 编码器。

为避免这些问题，请确保遵循 ES6 建议的不可变更新模式。

### 详细代码

更新复杂的嵌套状态树可能会导致冗长的代码编写繁琐且难以调试。

### 表现不佳

以不可变的方式操作 JavaScript 对象和数组可能会很慢，特别是在状态树变大时更是如此。

请记住，要更改不可变对象，必须对其*副本*进行变异，并且复制大对象可能会很慢，因为必须复制每个属性。

相比之下，Immutable.JS 等不可变的库可以采用复杂的优化技术，例如[结构共享](http://www.slideshare.net/mohitthatte/a-deep-dive-into-clojures-data-structures-euroclojure-2015)，它可以有效地返回一个新的对象，以重用大部分从中复制的现有对象。

对于复制非常大的对象，[普通JavaScript](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.z1g1ofrsi)比优化的不可变库[要慢100倍](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.z1g1ofrsi)以上。

#### 更多信息

**文档**

- Immutable Update Patterns for ES6（ES6不可更新的更新模式）

**文章**

- [Immutable.js, persistent data structures and structural sharing（](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.a2jimoiaf)Immutable.js，持久数据结构和结构共享[）](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.a2jimoiaf)
- [A deep dive into Clojure’s data structures（](http://www.slideshare.net/mohitthatte/a-deep-dive-into-clojures-data-structures-euroclojure-2015)深入探索Clojure的数据结构[）](http://www.slideshare.net/mohitthatte/a-deep-dive-into-clojures-data-structures-euroclojure-2015)
- [Introduction to Immutable.js and Functional Programming Concepts（](https://auth0.com/blog/intro-to-immutable-js/)Immutable.js和函数式编程概念介绍[）](https://auth0.com/blog/intro-to-immutable-js/)
- [JavaScript and Immutability（](http://t4d.io/javascript-and-immutability/)JavaScript和不可变性[）](http://t4d.io/javascript-and-immutability/)
- [Immutable Javascript using ES6 and beyond（](http://wecodetheweb.com/2016/02/12/immutable-javascript-using-es6-and-beyond/)使用ES6及更高版本的不可变JavaScript[）](http://wecodetheweb.com/2016/02/12/immutable-javascript-using-es6-and-beyond/)
- [Pros and Cons of using immutability with React.js - React Kung Fu（](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)React.js使用不变性的优点和缺点 - 反应功夫[）](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18