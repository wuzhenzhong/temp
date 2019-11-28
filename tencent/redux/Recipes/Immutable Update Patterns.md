# 不变的更新模式( Immutable Update Patterns )

- 贡献者1人

  

先决条件概念＃不可变数据管理中列出的文章提供了许多关于如何不变地执行基本更新操作的很好的示例，例如更新对象中的字段或将项添加到数组末尾。但是，减速器通常需要结合使用这些基本操作来执行更复杂的任务。以下是您可能必须实施的一些更常见任务的一些示例。

[纠错](javascript:;)

## 更新嵌套对象

要更新嵌套数据的关键是**在于每一个嵌套的等级必须被复制并适当地更新**。对于那些学习 Redux 的人来说，这通常是一个困难的概念，并且在尝试更新嵌套对象时经常发生一些特定的问题。这些会导致意外的直接突变，我们应该避免。

##### 常见错误＃1：指向相同对象的新变量

定义一个新的变量并**不会**创建一个新的实际的对象-它仅创建另一个引用同一个对象。这个错误的一个例子是：

```javascript
function updateNestedState(state, action) {
    let nestedState = state.nestedState;
    // ERROR: this directly modifies the existing object reference - don't do this!
    nestedState.nestedField = action.data;

    return {
        ...state,
        nestedState
    };
}
```

函数可以正确返回顶层状态对象的浅表副本，但由于`nestedState`变量仍指向现有对象，因此状态会直接发生变化。

##### 常见错误＃2：仅制作一个级别的浅表副本

此错误的另一个常见版本如下所示：

```javascript
function updateNestedState(state, action) {
    // Problem: this only does a shallow copy!
    let newState = {...state};

    // ERROR: nestedState is still the same object!
    newState.nestedState.nestedField = action.data;

    return newState;
}
```

制作顶级的浅表副本是**不**足够的`nestedState`对象也应该被复制。

##### 正确的方法：复制所有级别的嵌套数据

不幸的是，将不可变的更新正确应用于深度嵌套状态的过程很容易变得冗长而难以阅读。以下是更新`state.first.second[someId].fourthstate.first.second[someId].fourth`的示例：

```javascript
function updateVeryNestedField(state, action) {
    return {
        ...state,
        first : {
            ...state.first,
            second : {
                ...state.first.second,
                [action.someId] : {
                    ...state.first.second[action.someId],
                    fourth : action.someValue
                }
            }
        }
    }
}
```

显然，每一层嵌套使得阅读变得更加困难，并且给出更多犯错的机会。这是鼓励你保持你的状态变得平稳的几个原因之一，并且尽可能地组成减速器。

## 在数组中插入和删除项目

通常情况下，一个 Javascript 数组的内容使用的是变化的功能性`push`，`unshift`和`splice`。既然我们不想直接在 reducer 中改变状态，通常应该避免这些状态。因此，您可能会看到像这样写入“插入”或“删除”行为：

```javascript
function insertItem(array, action) {
    return [
        ...array.slice(0, action.index),
        action.item,
        ...array.slice(action.index)
    ]
}

function removeItem(array, action) {
    return [
        ...array.slice(0, action.index),
        ...array.slice(action.index + 1)
    ];
}
```

但是请记住，关键是**原始内存中的引用**不会被修改。**只要我们先制作一个副本，我们就可以安全地变更副本**。请注意，数组和对象都是如此，但嵌套值仍然必须使用相同的规则进行更新。

这意味着我们也可以像这样编写插入和删除函数：

```javascript
function insertItem(array, action) {
    let newArray = array.slice();
    newArray.splice(action.index, 0, action.item);
    return newArray;
}

function removeItem(array, action) {
    let newArray = array.slice();
    newArray.splice(action.index, 1);
    return newArray;
}
```

删除功能也可以实现为：

```javascript
function removeItem(array, action) {
    return array.filter( (item, index) => index !== action.index);
}
```

## 更新数组中的项目

更新数组中的一个项目可以通过使用`Array.map`，为我们想要更新的项目返回一个新值并返回所有其他项目的现有值来完成：

```javascript
function updateObjectInArray(array, action) {
    return array.map( (item, index) => {
        if(index !== action.index) {
            // This isn't the item we care about - keep it as-is
            return item;
        }

        // Otherwise, this is the one we want - return an updated value
        return {
            ...item,
            ...action.item
        };    
    });
}
```

## 不可变更新实用程序库

由于编写不可变更新代码会变得乏味，因此有许多实用程序库试图抽象出该过程。这些库在 API 和用法上各不相同，但都试图提供一种更简洁，更简洁的写这些更新的方式。有些像[dot-prop-immutable一样](https://github.com/debitoor/dot-prop-immutable)，为命令提供字符串路径：

```javascript
state = dotProp.set(state, `todos.${index}.complete`, true)
```

其他的，如 [immutability-helper ](https://github.com/kolodny/immutability-helper)（现在被弃用的React Immutability Helpers插件的一个分支），使用嵌套值和辅助函数：

```javascript
var collection = [1, 2, {a: [12, 17, 15]}];
var newCollection = update(collection, {2: {a: {$splice: [[1, 1, 13, 14]]}}});
```

它们可以为编写手动不可变的更新逻辑提供有用的替代方案。

在 [Redux Addons Catalog ](https://github.com/markerikson/redux-ecosystem-links)的 [不变数据不可变更新实用程序 ](https://github.com/markerikson/redux-ecosystem-links/blob/master/immutable-data.md#immutable-update-utilities)部分中可以找到许多不可变的更新实用程序的列表。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18