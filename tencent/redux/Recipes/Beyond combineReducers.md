# 超越式CombineReducers（Beyond combineReducers）

- 贡献者1人

  

Redux 中`combineReducers`包含的实用程序非常有用，但是限于处理单个常见用例：通过将更新每个片状态的工作委派给特定的片缩减器来更新一个纯 JavaScript 对象的状态树。它**不**处理其他使用情况，如 Immutable.js 地图由一个状态树，试图通过状态树的其他部分作为额外的参数到切片减速，或执行的切片减速调用“订购”。它也不关心给定的切片减速器如何工作。

那么常见的问题是“我怎样才能利用`combineReducers`处理这些其他用例？”。答案就是：“ 可能需要使用别的东西”。**一旦通过** `**combineReducers**`***\* 的核心用例**，使用更多“自定义”还原器逻辑**，无论是一次性用例的特定逻辑还是可广泛共享的可重用函数。以下是处理这些典型用例的一些建议，但可以随意提出自己的方法。

## 利用Immutable.js对象使用切片缩减器

由于`combineReducers`目前只适用于纯 Javascript 对象，因此使用 Immutable.js Map 对象作为其状态树顶部的应用程序无法用于`combineReducers`管理该 Map。由于许多开发人员都使用 Immutable.js ，因此有许多已发布的实用程序可提供等效的功能，如 [redux-immutable ](https://github.com/gajus/redux-immutable)。这个包提供了它自己的实现，`combineReducers`它知道如何迭代 Immutable Map 而不是一个简单的 Javascript 对象。

## 切片减速器之间共享数据

同样，如果为了处理某个特定行为`sliceReducerA`而需要某个`sliceReducerB`状态切片中的某些数据，或者`sliceReducerB`需要整个状态作为参数，`combineReducers`则不会自行处理。这可以通过编写一个自定义函数来解决，该函数知道在这些特定情况下将所需数据作为附加参数传递，例如：

```javascript
function combinedReducer(state, action) {
    switch(action.type) {
        case "A_TYPICAL_ACTION" : {
            return {
                a : sliceReducerA(state.a, action),
                b : sliceReducerB(state.b, action)
            };
        }
        case "SOME_SPECIAL_ACTION" : {
            return {
                // specifically pass state.b as an additional argument
                a : sliceReducerA(state.a, action, state.b),
                b : sliceReducerB(state.b, action)
            }        
        }
        case "ANOTHER_SPECIAL_ACTION" : {
            return {
                a : sliceReducerA(state.a, action),
                // specifically pass the entire state as an additional argument
                b : sliceReducerB(state.b, action, state)
            }         
        }    
        default: return state;
    }
}
```

“共享切片更新”问题的另一种替代方法是简单地将更多数据放入操作中。根据这个例子，这很容易用 thunk 函数或类似的方法完成：

```javascript
function someSpecialActionCreator() {
    return (dispatch, getState) => {
        const state = getState();
        const dataFromB = selectImportantDataFromB(state);

        dispatch({
            type : "SOME_SPECIAL_ACTION",
            payload : {
                dataFromB
            }
        });
    }
}
```

因为来自B切片的数据已经在动作中，所以减速器不必做任何特殊的事情来使数据`sliceReducerA`可用。

第三种方法是使用`combineReducers`生成的reducer 来处理“简单”情况，其中每个片缩减器可以独立更新，但也可以使用另一个 reducer 来处理数据需要跨片共享的“特殊”情况。然后，一个包装函数可以依次调用这两个reducer来生成最终结果：

```javascript
const combinedReducer = combineReducers({
    a : sliceReducerA,
    b : sliceReducerB
}); 

function crossSliceReducer(state, action) {
    switch(action.type) {
        case "SOME_SPECIAL_ACTION" : {
            return {
                // specifically pass state.b as an additional argument
                a : handleSpecialCaseForA(state.a, action, state.b),
                b : sliceReducerB(state.b, action)
            }        
        }
        default : return state;
    }
}

function rootReducer(state, action) {
    const intermediateState = combinedReducer(state, action);
    const finalState = crossSliceReducer(intermediateState, action);
    return finalState;
}
```

事实证明，有一个称为 [reduce-redurs ](https://github.com/acdlite/reduce-reducers)的有用实用程序可以使该过程更轻松。它只需要使用多个 reducer 并在`reduce()`上运行，并将中间状态值传递给下一个 reducer ：

```javascript
// Same as the "manual" rootReducer above
const rootReducer = reduceReducers(combinedReducers, crossSliceReducer);
```

请注意，如果您使用`reduceReducers`，则应确保列表中的第一个缩减器能够定义初始状态，因为后面的缩减器通常会假定整个状态已存在，而不是尝试提供默认值。

## 进一步建议

同样，重要的是要了解Redux减速器**只是**功能。虽然`combineReducers`很有用，但它只是工具箱中的一个工具。函数可以包含除switch语句以外的条件逻辑，函数可以被组合成相互包装，并且函数可以调用其他函数。也许你需要你的一个切片减速器能够重置其状态，并且只对整体的特定动作作出响应。你可以这样做：

```javascript
const undoableFilteredSliceA = compose(undoReducer, filterReducer("ACTION_1", "ACTION_2"), sliceReducerA);
const rootReducer = combineReducers({
    a : undoableFilteredSliceA,
    b : normalSliceReducerB
});
```

请注意，`combineReducers`不知道负责管理的reducer功能有什么特别之处`a`。我们不需要修改`combineReducers`撤消的东西 - 我们只是将我们需要的作品集成到一个新的组合函数中。

此外，尽管`combineReducers `Redux 中内置了一个 Reducer 实用程序功能，但仍有各种第三方 Reducer 实用程序已发布供重用。在[终极版附加组件目录](https://github.com/markerikson/redux-ecosystem-links)中列出的许多第三方工具可用。或者，如果没有任何已发布的实用程序解决您的使用案例，您可以自己编写一个完全符合您需要的功能。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18