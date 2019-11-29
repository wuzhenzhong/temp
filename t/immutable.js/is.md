# is()

值相等检查与语义类似`**Object.is**`，但将不可变`Iterable`作为值对待，如果第二个`Iterable`值包含等价值则相等。

```javascript
is(first: any, second: any): boolean
```

#### 讨论

它在整个不可变时用于检查相等性，包括`Map`键相等和`Set`成员资格。

[纠错](javascript:;)

```javascript
var map1 = Immutable.Map({a:1, b:1, c:1});
var map2 = Immutable.Map({a:1, b:1, c:1});
assert(map1 !== map2);
assert(Object.is(map1, map2) === false);
assert(Immutable.is(map1, map2) === true);
```

注：不像[`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)，`Immutable.is`假设`0`和`-0`值是相同的，匹配 ES6地图键平等的行为。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11