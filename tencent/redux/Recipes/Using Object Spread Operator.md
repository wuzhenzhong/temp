# 使用对象扩展运算符（Using Object Spread Operator）

- 贡献者1人

  

由于Redux的核心原则之一是永不改变状态，因此常常会发现自己使用的[`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)是创建具有新值或更新值的对象的副本。例如，在`todoApp`下面的代码`Object.assign()`中用于返回一个新的`state`对象与更新的`visibilityFilter`属性：

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

虽然有效，但`Object.assign()`由于其语法比较冗长，使用它可以快速地使简单的减速器难以阅读。

另一种方法是使用针对 JavaScript 的下一版本提出的[对象扩展语法](https://github.com/sebmarkbage/ecmascript-rest-spread)，该[语法](https://github.com/sebmarkbage/ecmascript-rest-spread)允许使用spread（`...`）运算符以更简洁的方式将可枚举的属性从一个对象复制到另一个对象。对象扩展运算符在概念上与 ES6 [阵列扩展运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)相似。我们可以`todoApp`通过使用对象扩展语法来简化上面的例子：

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return { ...state, visibilityFilter: action.filter }
    default:
      return state
  }
}
```

在编写复杂对象时，使用对象扩展语法的优点变得更加明显。下面`getAddedIds`的`id`值将数组映射到从`getProduct`and 返回值的对象数组`getQuantity`。

```javascript
return getAddedIds(state.cart).map(id =>
  Object.assign({}, getProduct(state.products, id), {
    quantity: getQuantity(state.cart, id)
  })
)
```

对象传播可以让我们简化上述`map`调用：

```javascript
return getAddedIds(state.cart).map(id => ({
  ...getProduct(state.products, id),
  quantity: getQuantity(state.cart, id)
}))
```

由于对象扩展语法仍然是 ECMAScript 的[第3阶段](https://github.com/sebmarkbage/ecmascript-rest-spread#status-of-this-proposal)提案，因此您需要使用像 [Babel ](http://babeljs.io/)这样的转译器才能在生产中使用它。您可以使用现有`es2015`预设，安装 [`babel-plugin-transform-object-rest-spread `](http://babeljs.io/docs/plugins/transform-object-rest-spread/)并将其单独添加到 `plugins `您的阵列中 `.babelrc`。

```javascript
{
  "presets": ["es2015"],
  "plugins": ["transform-object-rest-spread"]
}
```

请注意，这仍然是一项实验性语言功能建议，因此未来可能会发生变化。尽管如此，[React Native](https://github.com/facebook/react-native)等一些大型项目已经广泛使用它，所以可以肯定地说，如果它发生变化，将会有一个很好的自动迁移路径。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18