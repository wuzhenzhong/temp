# fromJS()

将纯 JS 对象和数组深层转换为不可变映射和列表。

#### 继承

```
Iterable#groupBy
```

#### 讨论

注意：这总是一个急切的操作。

## 副作用

### Collection.Keyed#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11