# 重用减速器逻辑（ Reusing Reducer Logic ）

- 贡献者1人

  

随着应用程序的增长，减速器逻辑中的常见模式将开始出现。您可能会发现 Reducer 逻辑的多个部分针对不同类型的数据执行相同类型的工作，并希望通过为每个数据类型重用相同的公共逻辑来减少重复。或者，您可能希望在商店中处理特定类型数据的多个“实例”。但是，Redux 商店的全局结构会带来一些折衷：它可以很容易地跟踪应用程序的整体状态，但也可能会使得难以“定位”需要更新特定状态的操作，特别是如果你正在使用`combineReducers`。

举一个例子，假设我们想跟踪应用程序中的多个计数器，分别命名为A，B和C。我们定义我们的初始`counter`缩减器，然后用`combineReducers`来设置我们的状态：

```javascript
function counter(state = 0, action) {
    switch (action.type) {
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
}

const rootReducer = combineReducers({
    counterA : counter,
    counterB : counter,
    counterC : counter
});
```

不幸的是，这个设置有问题。因为`combineReducers`将使用相同的动作调用每个slice reducer，调度`{type : 'INCREMENT'}`实际上会导致*所有三个*计数器值递增，而不仅仅是其中之一。我们需要一些方法来包装`counter`逻辑，以便确保只更新我们关心的计数器。

## 使用高阶减速器自定义行为

正如 Splitting Reducer Logic 中定义的那样，*高阶简化器*是一个将简化器函数作为参数的函数，并且/或者返回一个新的简化器函数作为结果。它也可以被看作是“减速器工厂”。`combineReducers`是高阶减速器的一个例子。我们可以使用这种模式来创建我们自己的reducer功能的专用版本，每个版本只响应特定的操作。

专门化 Reducer 的两种最常见的方法是使用给定的前缀或后缀生成新的操作常量，或者在操作对象中附加附加信息。以下是这些可能的样子：

```javascript
function createCounterWithNamedType(counterName = '') {
    return function counter(state = 0, action) {
        switch (action.type) {
            case `INCREMENT_${counterName}`:
                return state + 1;
            case `DECREMENT_${counterName}`:
                return state - 1;
            default:
                return state;
        }
    }
}

function createCounterWithNameData(counterName = '') {
    return function counter(state = 0, action) {
        const {name} = action;
        if(name !== counterName) return state;

        switch (action.type) {
            case `INCREMENT`:
                return state + 1;
            case `DECREMENT`:
                return state - 1;
            default:
                return state;
        }
    }
}
```

我们现在应该能够使用其中任何一种来生成我们专门的计数器减速器，然后分派将影响我们关心的部分状态的操作：

```javascript
const rootReducer = combineReducers({
    counterA : createCounterWithNamedType('A'),
    counterB : createCounterWithNamedType('B'),
    counterC : createCounterWithNamedType('C'),
});

store.dispatch({type : 'INCREMENT_B'});
console.log(store.getState());
// {counterA : 0, counterB : 1, counterC : 0}
```

我们也可以在某种程度上改变这种方法，并创建一个更通用的高阶简化器，它接受给定的简化器函数和名称或标识符：

```javascript
function counter(state = 0, action) {
    switch (action.type) {
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
}

function createNamedWrapperReducer(reducerFunction, reducerName) {
    return (state, action) => {
        const {name} = action;
        const isInitializationCall = state === undefined;
        if(name !== reducerName && !isInitializationCall) return state;

        return reducerFunction(state, action);    
    }
}

const rootReducer = combineReducers({
    counterA : createNamedWrapperReducer(counter, 'A'),
    counterB : createNamedWrapperReducer(counter, 'B'),
    counterC : createNamedWrapperReducer(counter, 'C'),
});
```

你甚至可以去做一个通用的过滤高阶简化器：

```javascript
function createFilteredReducer(reducerFunction, reducerPredicate) {
    return (state, action) => {
        const isInitializationCall = state === undefined;
        const shouldRunWrappedReducer = reducerPredicate(action) || isInitializationCall;
        return shouldRunWrappedReducer ? reducerFunction(state, action) : state;
    }
}

const rootReducer = combineReducers({
    // check for suffixed strings
    counterA : createFilteredReducer(counter, action => action.type.endsWith('_A')),
    // check for extra data in the action
    counterB : createFilteredReducer(counter, action => action.name === 'B'),
    // respond to all 'INCREMENT' actions, but never 'DECREMENT'
    counterC : createFilteredReducer(counter, action => action.type === 'INCREMENT')
};
```

这些基本模式允许您执行如下操作：在UI中使用智能连接组件的多个实例，或重用常用逻辑以实现分页或排序等通用功能。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18