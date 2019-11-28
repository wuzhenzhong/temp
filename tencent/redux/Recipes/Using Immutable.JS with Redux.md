# 使用带Redux的Immutable.JS（Using Immutable.JS with Redux）

- 贡献者1人

  

## 目录

- 为什么我应该使用 Immutable.JS 等不可变的库？

- 为什么我应该选择 Immutable.JS 作为不可变的库？

- 使用 Immutable.JS 有什么问题？

- Immutable.JS 值得付出的努力吗？

- 什么是使用 Immutable.JS 和 Redux 时一些死板的最佳实践？

## 为什么我应该使用Immutable.JS等不可变的库？

像 Immutable.JS 这样不可变性的库被设计用于克服 JavaScript 中固有的不变性问题，通过应用程序所需的性能提供不变性的所有优点。

无论您选择使用这样的库还是坚持使用普通的 JavaScript ，取决于您对应用程序添加其他依赖程序的自如程度，或者确信自己可以避免 JavaScript 不可变性方法中固有的缺陷。

无论您选择哪种方案，都要确保您熟悉不变性，副作用和变异的概念。特别是，确保您深入了解JavaScript在更新和复制值时所做的操作，以防止意外的突变，从而降低应用程序的性能，或完全破坏应用程序的性能。

#### 更多信息

**文档**

- Recipes：不变性，副作用和突变**Articles**

- [Immutable.js和函数式编程概念介绍](https://auth0.com/blog/intro-to-immutable-js/)

- [React.js使用不变性的优点和缺点](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

## 为什么我应该选择 Immutable.JS 作为不可变的库？

Immutable.JS 旨在以一种高性能的方式提供不变性，以努力克服 JavaScript 不可变性的局限性。其主要优势包括：

#### 保证不变性

封装在 Immutable.JS 对象中的数据永远不会发生变异。新副本总是被返回。这与 JavaScript 相反，其中一些操作不会改变数据（例如，某些数组方法，包括 map，filter，concat，forEach 等），但有一些操作（数组的弹出，推送，拼接等）则会。

#### Rich API

Immutable.JS 提供了一组丰富的不可变对象来封装数据（例如，地图，列表，集合，记录等）以及一系列操作它的方法，包括对数据进行排序，过滤和分组的方法，扭转，压缩，并创建子集。

#### Performance

Immutable.JS 在后台做了很多工作来优化性能。这是其能力的关键，因为使用不可变数据结构可能涉及大量昂贵的复制。特别是，不断修改大量复杂的数据集（如嵌套的 Redux 状态树）可以生成许多对象的中间副本，这些副本会消耗内存并降低性能，因为浏览器的垃圾收集器会对其进行清理。

Immutable.JS 通过在表面之下[共享数据结构](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.z1g1ofrsi)避免了这种情况，最大限度地减少了复制数据的需要。它还可以执行复杂的操作链，而不会产生不必要的（并且成本高昂）复制的中间数据，这些数据很快就会被丢弃。

当然，你永远不会看到这些 - 你给 ImmutableJS 对象的数据永远不会发生变化。相反，它是 Immutable.JS 中从一系列可以自由变异的方法调用的链式生成的*中间*数据。因此，您可以获得不变数据结构的所有优势，而不会产生任何潜在性能问题（或很少）。

#### 更多信息

**Articles**

- [Immutable.js，持久数据结构和结构共享](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.6nwctunlc)

- [PDF：JavaScript不可变性 - 不要改变](https://www.jfokus.se/jfokus16/preso/JavaScript-Immutability--Dont-Go-Changing.pdf)

**Libraries**

- [Immutable.js, persistent data structures and structural sharing](https://facebook.github.io/immutable-js/)What are the issues with using Immutable.JS?Although powerful, Immutable.JS needs to be used carefully, as it comes with issues of its own. Note, however, that all of these issues can be overcome quite easily with careful coding.Difficult to interoperate withJavaScript does not provide immutable data structures. As such, for Immutable.JS to provide its immutable guarantees, your data must be encapsulated within an Immutable.JS object (such as a `Map` or a `List`, etc.). Once it’s contained in this way, it’s hard for that data to then interoperate with other, plain JavaScript objects.For example, you will no longer be able to reference an object’s properties through standard JavaScript dot or bracket notation. Instead, you must reference them via Immutable.JS’s `get()` or `getIn()` methods, which use an awkward syntax that accesses properties via an array of strings, each of which represents a property key.For example, instead of `myObj.prop1.prop2.prop3`, you would use `myImmutableMap.getIn([‘prop1’, ‘prop2’, ‘prop3’])`.This makes it awkward to interoperate not just with your own code, but also with other libraries, such as lodash or ramda, that expect plain JavaScript objects.Note that Immutable.JS objects do have a `toJS()` method, which returns the data as a plain JavaScript data structure, but this method is extremely slow, and using it extensively will negate the performance benefits that Immutable.JS providesOnce used, Immutable.JS will spread throughout your codebaseOnce you encapsulate your data with Immutable.JS, you have to use Immutable.JS’s `get()` or `getIn()` property accessors to access it.This has the effect of spreading Immutable.JS across your entire codebase, including potentially your components, where you may prefer not to have such external dependencies. Your entire codebase must know what is, and what is not, an Immutable.JS object. It also makes removing Immutable.JS from your app difficult in the future, should you ever need to.This issue can be avoided by [uncoupling your application logic from your data structures](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.z1g1ofrsi), as outlined in the best practices section below.No Destructuring or Spread OperatorsBecause you must access your data via Immutable.JS’s own `get()` and `getIn()` methods, you can no longer use JavaScript’s destructuring operator (or the proposed Object spread operator), making your code more verbose.Not suitable for small values that change oftenImmutable.JS works best for collections of data, and the larger the better. It can be slow when your data comprises lots of small, simple JavaScript objects, with each comprising a few keys of primitive values.Note, however, that this does not apply to the Redux state tree, which is (usually) represented as a large collection of data.Difficult to DebugImmutable.JS objects, such as `Map`, `List`, etc., can be difficult to debug, as inspecting such an object will reveal an entire nested hierarchy of Immutable.JS-specific properties that you don’t care about, while your actual data that you do care about is encapsulated several layers deep.To resolve this issue, use a browser extension such as the [Immutable.js Object Formatter](https://chrome.google.com/webstore/detail/immutablejs-object-format/hgldghadipiblonfkkicmgcbbijnpeog), which surfaces your data in Chrome Dev Tools, and hides Immutable.JS’s properties when inspecting your data.Breaks object references, causing poor performanceOne of the key advantages of immutability is that it enables shallow equality checking, which dramatically improves performance.If two different variables reference the same immutable object, then a simple equality check of the two variables is enough to determine that they are equal, and that the object they both reference is unchanged. The equality check never has to check the values of any of the object’s properties, as it is, of course, immutable.However, shallow checking will not work if your data encapsulated within an Immutable.JS object is itself an object. This is because Immutable.JS’s `toJS()` method, which returns the data contained within an Immutable.JS object as a JavaScript value, will create a new object every time it’s called, and so break the reference with the encapsulated data.Accordingly, calling `toJS()` twice, for example, and assigning the result to two different variables will cause an equality check on those two variables to fail, even though the object values themselves haven’t changed.This is a particular issue if you use `toJS()` in a wrapped component’s `mapStateToProps` function, as React-Redux shallowly compares each value in the returned props object. For example, the value referenced by the `todos` prop returned from `mapStateToProps` below will always be a different object, and so will fail a shallow equality check.// AVOID .toJS() in mapStateToProps function mapStateToProps(state) { return { todos: state.get('todos').toJS() // Always a new object } }When the shallow check fails, React-Redux will cause the component to re-render. Using `toJS()` in `mapStateToProps` in this way, therefore, will always cause the component to re-render, even if the value never changes, impacting heavily on performance.This can be prevented by using `toJS()` in a Higher Order Component, as discussed in the Best Practices section below.Further Information**Articles**

- [Immutable.js, persistent data structures and structural sharing](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2#.hzgz7ghbe)

- [Immutable Data Structures and JavaScript](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript)

- [React.js pure render performance anti-pattern](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.9ucv6hwk4)

- [Building Efficient UI with React and Redux](https://www.toptal.com/react/react-redux-and-immutablejs)

**Chrome Extension**

- [Immutable Object Formatter](https://chrome.google.com/webstore/detail/immutablejs-object-format/hgldghadipiblonfkkicmgcbbijnpeog)Is Using Immutable.JS worth the effort?Frequently, yes. There are various tradeoffs and opinions to consider, but there are many good reasons to use Immutable.JS. Do not underestimate the difficulty of trying to track down a property of your state tree that has been inadvertently mutated.Components will both re-render when they shouldn’t, and refuse to render when they should, and tracking down the bug causing the rendering issue is hard, as the component rendering incorrectly is not necessarily the one whose properties are being accidentally mutated.This problem is caused predominantly by returning a mutated state object from a Redux reducer. With Immutable.JS, this problem simply does not exist, thereby removing a whole class of bugs from your app.This, together with its performance and rich API for data manipulation, is why Immutable.JS is worth the effort.Further Information**Documentation**

- Troubleshooting: Nothing happens when I dispatch an action

## What are some opinionated Best Practices for using Immutable.JS with Redux?

Immutable.JS can provide significant reliability and performance improvements to your app, but it must be used correctly. If you choose to use Immutable.JS (and remember, you are not required to, and there are other immutable libraries you can use), follow these opinionated best practices, and you’ll be able to get the most out of it, without tripping up on any of the issues it can potentially cause.

### Never mix plain JavaScript objects with Immutable.JS

Never let a plain JavaScript object contain Immutable.JS properties. Equally, never let an Immutable.JS object contain a plain JavaScript object.

#### Further Information

**Articles**

- [Immutable Data Structures and JavaScript](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript)Make your entire Redux state tree an Immutable.JS objectFor a Redux app, your entire state tree should be an Immutable.JS object, with no plain JavaScript objects used at all.

- Create the tree using Immutable.JS’s `fromJS()` function.

- Use an Immutable.JS-aware version of the `combineReducers` function, such as the one in [redux-immutable](https://www.npmjs.com/package/redux-immutable), as Redux itself expects the state tree to be a plain JavaScript object.

- When adding JavaScript objects to an Immutable.JS Map or List using Immutable.JS’s `update`, `merge` or `set` methods, ensure that the object being added is first converted to an Immutable object using `fromJS()`.

**Example**

```javascript
// avoid
const newObj = { key: value }
const newState = state.setIn(['prop1'], newObj)
// newObj has been added as a plain JavaScript object, NOT as an Immutable.JS Map

// recommended
const newObj = { key: value }
const newState = state.setIn(['prop1'], fromJS(newObj))
// newObj is now an Immutable.JS Map
```

#### Further Information

**Articles**

- [Immutable Data Structures and JavaScript](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript)**Libraries**

- [redux-immutable](https://www.npmjs.com/package/redux-immutable)

### Use Immutable.JS everywhere except your dumb components

Using Immutable.JS everywhere keeps your code performant. Use it in your smart components, your selectors, your sagas or thunks, action creators, and especially your reducers.

Do not, however, use Immutable.JS in your dumb components.

#### Further Information

**Articles**

- [Immutable Data Structures and JavaScript](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript)

- [Smart and Dumb Components in React](http://jaketrent.com/post/smart-dumb-components-react/)

### Limit your use of `toJS()`

`toJS()` is an expensive function and negates the purpose of using Immutable.JS. Avoid its use.

#### Further Information

**Discussions**

- [Lee Byron on Twitter: “Perf tip for #immutablejs…”](https://twitter.com/leeb/status/746733697093668864)Your selectors should return Immutable.JS objectsAlways.Use Immutable.JS objects in your Smart ComponentsSmart components that access the store via React Redux’s `connect` function must use the Immutable.JS values returned by your selectors. Make sure you avoid the potential issues this can cause with unnecessary component re-rendering. Memoize your selectors using a library such as reselect if necessary.Further Information**Documentation**

- Recipes: Computing Derived Data

- FAQ: Immutable Data

- [Reselect Documentation: How do I use Reselect with Immutable.js?](https://github.com/reactjs/reselect/#q-how-do-i-use-reselect-with-immutablejs)

**Articles**

- [Redux Patterns and Anti-Patterns](https://tech.affirm.com/redux-patterns-and-anti-patterns-7d80ef3d53bc#.451p9ycfy)**Libraries**

- [Reselect: Selector library for Redux](https://github.com/reactjs/reselect)

### Never use `toJS()` in `mapStateToProps`

Converting an Immutable.JS object to a JavaScript object using `toJS()` will return a new object every time. If you do this in `mapStateToProps`, you will cause the component to believe that the object has changed every time the state tree changes, and so trigger an unnecessary re-render.

#### Further Information

**Documentation**

- FAQ: Immutable DataNever use Immutable.JS in your Dumb ComponentsYour dumb components should be pure; that is, they should produce the same output given the same input, and have no external dependencies. If you pass such a component an Immutable.JS object as a prop, you make it dependent upon Immutable.JS to extract the prop’s value and otherwise manipulate it.Such a dependency renders the component impure, makes testing the component more difficult, and makes reusing and refactoring the component unnecessarily difficult.Further Information**Articles**

- [Immutable Data Structures and JavaScript](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript)

- [Smart and Dumb Components in React](http://jaketrent.com/post/smart-dumb-components-react/)

- [Tips For a Better Redux Architecture: Lessons for Enterprise Scale](https://hashnode.com/post/tips-for-a-better-redux-architecture-lessons-for-enterprise-scale-civrlqhuy0keqc6539boivk2f)

### Use a Higher Order Component to convert your Smart Component’s Immutable.JS props to your Dumb Component’s JavaScript props

Something needs to map the Immutable.JS props in your Smart Component to the pure JavaScript props used in your Dumb Component. That something is a Higher Order Component (HOC) that simply takes the Immutable.JS props from your Smart Component, and converts them using `toJS()` to plain JavaScript props, which are then passed to your Dumb Component.

Here is an example of such a HOC:

```javascript
import React from 'react'
import { Iterable } from 'immutable'

export const toJS = WrappedComponent => wrappedComponentProps => {
  const KEY = 0
  const VALUE = 1

  const propsJS = Object.entries(
    wrappedComponentProps
  ).reduce((newProps, wrappedComponentProp) => {
    newProps[wrappedComponentProp[KEY]] = Iterable.isIterable(
      wrappedComponentProp[VALUE]
    )
      ? wrappedComponentProp[VALUE].toJS()
      : wrappedComponentProp[VALUE]
    return newProps
  }, {})

  return <WrappedComponent {...propsJS} />
}
```

And this is how you would use it in your Smart Component:

```javascript
import { connect } from 'react-redux'

import { toJS } from './to-js'
import DumbComponent from './dumb.component'

const mapStateToProps = state => {
  return {
    // obj is an Immutable object in Smart Component, but it’s converted to a plain
    // JavaScript object by toJS, and so passed to DumbComponent as a pure JavaScript
    // object. Because it’s still an Immutable.JS object here in mapStateToProps, though,
    // there is no issue with errant re-renderings.
    obj: getImmutableObjectFromStateTree(state)
  }
}
export default connect(mapStateToProps)(toJS(DumbComponent))
```

By converting Immutable.JS objects to plain JavaScript values within a HOC, we achieve Dumb Component portability, but without the performance hits of using `toJS()` in the Smart Component.

*Note: if your app requires high performance, you may need to avoid* *toJS()* *altogether, and so will have to use Immutable.JS in your dumb components. However, for most apps this will not be the case, and the benefits of keeping Immutable.JS out of your dumb components (maintainability, portability and easier testing) will far outweigh any perceived performance improvements of keeping it in.*

*In addition, using* *toJS* *in a Higher Order Component should not cause much, if any, performance degradation, as the component will only be called when the connected component’s props change. As with any performance issue, conduct performance checks first before deciding what to optimise.*

#### Further Information

**Documentation**

- [React: Higher-Order Components](https://facebook.github.io/react/docs/higher-order-components.html)**Articles**

- [React Higher Order Components in depth](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e#.dw2qd1o1g)

**Discussions**

- [Reddit: acemarke and cpsubrian comments on Dan Abramov: Redux is not an architecture or design pattern, it is just a library.](https://www.reddit.com/r/javascript/comments/4rcqpx/dan_abramov_redux_is_not_an_architecture_or/d5rw0p9/?context=3)**Gists**

- [cpsubrian: React decorators for redux/react-router/immutable ‘smart’ components](https://gist.github.com/cpsubrian/79e97b6116ab68bd189eb4917203242c#file-tojs-js)

### Use the Immutable Object Formatter Chrome Extension to Aid Debugging

Install the [Immutable Object Formatter](https://chrome.google.com/webstore/detail/immutablejs-object-format/hgldghadipiblonfkkicmgcbbijnpeog) , and inspect your Immutable.JS data without seeing the noise of Immutable.JS's own object properties.

#### Further Information

**Chrome Extension**

- [Immutable Object Formatter](https://chrome.google.com/webstore/detail/immutablejs-object-format/hgldghadipiblonfkkicmgcbbijnpeog)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18