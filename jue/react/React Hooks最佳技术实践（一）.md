# React Hooks最佳技术实践（一）

> 原文地址：[博客](https://link.juejin.im/?target=https%3A%2F%2Fwww.linxiangjun.com%2F2019%2F10%2F15%2Freact-hooks-best-practices-1%2F%23%E5%89%8D%E8%A8%80)

## 前言

React 16.8版本发布于2019年2月6号，它带来了Hooks特性，能够让我们不编写class的情况下也能使用state以及其他的React特性。一个新技术的诞生必然会影响原来的思维模式，在最初的应用中也会碰见很多意想不到的场景和陷阱，这就需要我们在使用他们前清楚的知道哪里可能会有潜在的问题，有哪些更加完善的写法，这也是写本文章的初衷。我们团队在7月份开始在新项目中全面使用Hooks技术，期间也遇到和解决了很多的麻烦和问题，同时让我们对于函数式组件有了更深的理解。

在本文开始前，需要你掌握React官方中关于[React Hooks](https://link.juejin.im/?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Fhooks-intro.html)的基础知识。很多坑在官方的[FAQ](https://link.juejin.im/?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Fhooks-faq.html)已经说明了，经常阅读官方文档也是个良好的习惯。

本文介绍的是我在使用Hooks的过程中遇见的问题和技巧，可能还有其他我没发现的或者没有写出来的，欢迎和我交流，希望通过阅读本文能够让你快速的掌握Hooks的陷阱和技巧，开发出高质量的函数式组件。那我们就先从`useState`开始。

## useState

在Class Component中，使用state来存储组件的状态，用`setState`来更新，在Hooks中提供了`useState`API来创建一个state。

### state结构

`useState`接收一个initialState，同时返回一个state以及更新state的函数。initialState可以使用基础数据类型，也可以使用数组、对象等引用数据类型。这里建议根据state的关联度和作用来决定是否使用多个`useState`来定义state。

在下面的例子中，我们根据后端接口返回的数据`{data: [Tom, Jerry], total: 2, result: true}`来设置state数据。第一种写法直接将所有的请求参数和返回数据全部存储到一个`userState`中，我们来分析下为何不推荐这么做。

首先，返回的数据中存在不需要存储在state中的数据字段result。其次，将data与requestParams放在一个state中看似简单，只需要维护一个`userState`，但是如果需要在`useEffect`中依赖`data`或者`requestParams`将会非常的麻烦，而且每次获取后端返回的数据都将需要一同更新`requestParams`。

**不建议的写法**

```
const [userState, setUserState] = useState({
    data: [],
    total: 0,
    requestParams: { id: 1 },
    result: true
})
复制代码
```

**推荐的写法**

```
const [userData, setUserData] = useState({ data: [], total: 0 })
const [userParams, setUserParams] = useState({ id: 1 })
复制代码
```

### 避免重复计算

如果initialState为函数，则`useState`在初始化时会立刻执行该函数和获取函数的返回值，在没有任何返回值得情况下为`undefined`。这里需要注意的是每次组件re-render都会导致`useState`中的函数重新计算，这里可以使用闭包函数来解决问题。在优化后只有组件初始化时才会执行一遍`loop`函数。

本例子可以在[CodeSandbox](https://link.juejin.im/?target=https%3A%2F%2Fcodesandbox.io%2Fs%2Fgracious-stallman-j3zuw%3Ffontsize%3D14)中查看

**优化前**

```
const loop = () => {
  console.log("calc!");
  let res = 0;
  for (let i = 0, len = 1000; i < len; i++) {
    res += i;
  }
  return res;
};

const [value, setValue] = useState(loop());
复制代码
```

**优化后**

```
const App = () => {
  const [value, setValue] = useState(() => {
    return loop();
  });
}
复制代码
```

### 更新state

在某些情况下如果需要从上一个state来计算当前的state，可能会想到使用下面优化前的方法。但是，需要注意的一点是在某些闭包的场景下面`count`可能不是最新的，这样会导致计算错误。这里推荐使用官方给出的方法。

**优化前**

```
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>+</button>
}
复制代码
```

**优化后**

```
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
}
复制代码
```

下一篇文章介绍`useEffect`和`useLayoutEffect`的使用技巧。