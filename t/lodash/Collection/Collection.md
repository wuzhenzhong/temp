# Collection

- 贡献者1人

  

### _.countBy(collection, iteratee=_.identity)

创建一个由运行集合中的每个元素到迭代器的结果生成的键组成的对象。每个键的相应值是密钥返回的次数`iteratee`。迭代器调用一个参数：*（value）*。

##### 初始

0.5.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：用于转换密钥的迭代器。

##### 返回

*（Object）*：返回组合的聚合对象。

##### 例

```javascript
_.countBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': 1, '6': 2 }
 
// The `_.property` iteratee shorthand.
_.countBy(['one', 'two', 'three'], 'length');
// => { '3': 2, '5': 1 }
```

### _.every(collection, predicate=_.identity)

检查是否对于`collection`**所有**元素,`predicate`返回true 。一旦`predicate`返回falsey，迭代就会停止。谓词用三个参数调用：*（value，index | key，collection）*。

**注意：**此方法返回`true`的[空集](https://en.wikipedia.org/wiki/Empty_set)，因为是[一切都是真的](https://en.wikipedia.org/wiki/Vacuous_truth)空集的元素。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（boolean）*：所有元素是否通过了谓词检查则返回`true`，否则`false`。

##### 例

```javascript
_.every([true, 1, null, 'yes'], Boolean);
// => false
 

var users = [
  { 'user': 'barney', 'age': 36, 'active': false },

  { 'user': 'fred',   'age': 40, 'active': false }
];
 
// The `_.matches` iteratee shorthand.
_.every(users, { 'user': 'barney', 'active': false });
// => false
 
// The `_.matchesProperty` iteratee shorthand.
_.every(users, ['active', false]);
// => true
 
// The `_.property` iteratee shorthand.
_.every(users, 'active');
// => false
```

### _.filter(collection, predicate=_.identity)

迭代集合的元素，返回谓词，返回truthy的所有元素的数组。谓词用三个参数调用：*（value，index | key，collection）*。

**注意：** 与`_.remove`此不同，此方法返回一个新数组。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回新的过滤数组。

##### 例

```javascript
var users = [
  { 'user': 'barney', 'age': 36, 'active': true },

  { 'user': 'fred',   'age': 40, 'active': false }
];
 
_.filter(users, function(o) { return !o.active; });
// => objects for ['fred']
 
// The `_.matches` iteratee shorthand.
_.filter(users, { 'age': 36, 'active': true });
// => objects for ['barney']
 
// The `_.matchesProperty` iteratee shorthand.
_.filter(users, ['active', false]);
// => objects for ['fred']
 
// The `_.property` iteratee shorthand.
_.filter(users, 'active');
// => objects for ['barney']
```

### _.find(collection, predicate=_.identity, fromIndex=0)

迭代集合的元素，返回第一个元素谓词，返回truthy。 谓词用三个参数调用：（value，index | key，collection）。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：要检查的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[fromIndex=0]` *（number）*：从中搜索的索引。

##### 返回

*（\*）*：返回匹配的元素，否则返回`undefined`。

##### 例

```javascript
var users = [
  { 'user': 'barney',  'age': 36, 'active': true },

  { 'user': 'fred',    'age': 40, 'active': false },

  { 'user': 'pebbles', 'age': 1,  'active': true }
];
 
_.find(users, function(o) { return o.age < 40; });
// => object for 'barney'
 
// The `_.matches` iteratee shorthand.
_.find(users, { 'age': 1, 'active': true });
// => object for 'pebbles'
 
// The `_.matchesProperty` iteratee shorthand.
_.find(users, ['active', false]);
// => object for 'fred'
 
// The `_.property` iteratee shorthand.
_.find(users, 'active');
// => object for 'barney'
```

### _.findLast(collection, predicate=_.identity, fromIndex=collection.length-1)

这个方法就像`_.find`，不同的是它遍历`collection`从右到左的元素 。

##### 初始

2.0.0

##### 参数

1. `collection` *（Array | Object）*：要检查的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[fromIndex=collection.length-1]` *（number）*：从中搜索的索引。

##### 返回

*（\*）*：返回匹配的元素，否则返回`undefined`。

##### 例

```javascript
_.findLast([1, 2, 3, 4], function(n) {
  return n % 2 == 1;
});
// => 3
```

### _.flatMap(collection, iteratee=_.identity)

通过在迭代中运行collection中的每个元素并压扁映射的结果来创建一个扁平化的值数组。迭代器被调用三个参数：*（value，index | key，collection）*。

##### 初始

4.0.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回新的展平数组。

##### 例

```javascript
function duplicate(n) {
  return [n, n];
}
 
_.flatMap([1, 2], duplicate);
// => [1, 1, 2, 2]
```

### _.flatMapDeep(collection, iteratee=_.identity)

这种方法就像`_.flatMap`，不同的是它递归地平整映射结果。

##### 初始

4.7.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回新的展平数组。

##### 例

```javascript
function duplicate(n) {
  return [[[n, n]]];
}
 
_.flatMapDeep([1, 2], duplicate);
// => [1, 1, 2, 2]
```

### _.flatMapDepth(collection, iteratee=_.identity, depth=1)

这种方法就像`_.flatMap`,不同的是它递归地将映射的结果平展到`depth`时间。

##### 初始

4.7.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[depth=1]` *（数字）*：最大递归深度。

##### 返回

*（数组）*：返回新的展平数组。

##### 例

```javascript
function duplicate(n) {
  return [[[n, n]]];
}
 
_.flatMapDepth([1, 2], duplicate, 2);
// => [[1, 1], [2, 2]]
```

### _.forEach(collection, iteratee=_.identity)

迭代元素`collection`与`iteratee`为每个元素调用。迭代器调用三个参数：*（value，index | key，collection）*。迭代器函数可以通过显式提前退出迭代，返回`false`。

**注意：**与其他“集合”方法一样，具有“长度”属性的对象与数组一样迭代。为了避免这种行为，使用`_.forIn`或`_.forOwn`用于对象迭代。

##### 初始

0.1.0

##### 别名

*_.each*

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*(\*)*: 返回 `collection`.

##### 例

```javascript
_.forEach([1, 2], function(value) {
  console.log(value);
});
// => Logs `1` then `2`.
 
_.forEach({ 'a': 1, 'b': 2 }, function(value, key) {
  console.log(key);
});
// => Logs 'a' then 'b' (iteration order is not guaranteed).
```

### _.forEachRight(collection, iteratee=_.identity)

这个方法就像`_.forEach`，不同的是它遍历`collection` 从右到左的元素。

##### 初始

2.0.0

##### 别名

*_.eachRight*

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*(\*)*: 返回`collection`.

##### 例

```javascript
_.forEachRight([1, 2], function(value) {
  console.log(value);
});
// => Logs `2` then `1`.
```

### _.groupBy(collection, iteratee=_.identity)

创建一个由运行集合中的每个元素到迭代器的结果生成的键组成的对象。 分组值的顺序由它们在收集中发生的顺序决定。每个键的相应值是负责生成键的元素数组。迭代器被调用一个参数：*（value）*。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（函数）*：用于转换密钥的迭代器。

##### 返回

*（Object）*：返回组合的聚合对象。

##### 例

```javascript
_.groupBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': [4.2], '6': [6.1, 6.3] }
 
// The `_.property` iteratee shorthand.
_.groupBy(['one', 'two', 'three'], 'length');
// => { '3': ['one', 'two'], '5': ['three'] }
```

### _.includes(collection, value, fromIndex=0)

检查值是否在集合中。 如果collection是一个字符串，则检查是否有值的子字符串，否则使用SameValueZero进行相等性比较。 如果fromIndex为负数，它将用作从收集结束的偏移量。

##### **初始**



0.1.0

##### 参数

1. `collection` *（Array | Object | string）*：要检查的集合。
2. `value` *（\*）*：要搜索的值。
3. `[fromIndex=0]` *（数字）*：从中搜索的索引。

##### 返回

*(boolean)*: 如果找到值，则返回true，否则返回false。

##### 例

```javascript
_.includes([1, 2, 3], 1);
// => true
 
_.includes([1, 2, 3], 1, 2);
// => false
 
_.includes({ 'a': 1, 'b': 2 }, 1);
// => true
 
_.includes('abcd', 'bc');
// => true
```

### _.invokeMap(collection, path, args)

在集合中每个元素的路径上调用该方法，返回每个调用方法的结果数组。 任何额外的参数都提供给每个调用的方法。 如果path是一个函数，那么它将被调用并绑定到集合中的每个元素。

##### 初始

4.0.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `path` *（Array | Function | string）*：要调用的方法的路径或每次迭代调用的函数。
3. `[args]` *（... \*）*：用来调用每个方法的参数。

##### 返回

*（数组）*：返回结果数组。

##### 例

```javascript
_.invokeMap([[5, 1, 7], [3, 2, 1]], 'sort');
// => [[1, 5, 7], [1, 2, 3]]
 
_.invokeMap([123, 456], String.prototype.split, '');
// => [['1', '2', '3'], ['4', '5', '6']]
```

### _.keyBy(collection, iteratee=_.identity)

创建一个由运行集合中的每个元素到迭代器的结果生成的键组成的对象。 每个键的相应值是负责生成密钥的最后一个元素。 迭代器被调用一个参数：（value）。

##### 初始

4.0.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（函数）*：用于转换密钥的迭代器。

##### 返回

*（Object）*：返回组合的聚合对象。

##### 例

```javascript
var array = [
  { 'dir': 'left', 'code': 97 },

  { 'dir': 'right', 'code': 100 }
];
 
_.keyBy(array, function(o) {
  return String.fromCharCode(o.code);
});
// => { 'a': { 'dir': 'left', 'code': 97 }, 'd': { 'dir': 'right', 'code': 100 } }
 
_.keyBy(array, 'dir');
// => { 'left': { 'dir': 'left', 'code': 97 }, 'right': { 'dir': 'right', 'code': 100 } }
```

### _.map(collection, iteratee=_.identity)

通过运行iteratee中的每个元素来创建一个值的数组。 迭代器被调用三个参数：

*(value, index|key, collection)*.

可以用许多lodash方法防护，iteratees工作如下：`_.every`，`_.filter`， `_.map`，`_.mapValues`，`_.reject`，和`_.some`。

防护的方法是：

```
ary`, `chunk`, `curry`, `curryRight`, `drop`, `dropRight`, `every`, `fill`, `invert`, `parseInt`, `random`, `range`, `rangeRight`, `repeat`, `sampleSize`, `slice`, `some`, `sortBy`, `split`, `take`, `takeRight`, `template`, `trim`, `trimEnd`, `trimStart`, and `words
```

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回新的映射数组。

##### 例

```javascript
function square(n) {
  return n * n;
}
 
_.map([4, 8], square);
// => [16, 64]
 
_.map({ 'a': 4, 'b': 8 }, square);
// => [16, 64] (iteration order is not guaranteed)
 

var users = [
  { 'user': 'barney' },

  { 'user': 'fred' }
];
 
// The `_.property` iteratee shorthand.
_.map(users, 'user');
// => ['barney', 'fred']
```

### _.orderBy(collection, [iteratees=_.identity], orders)

此方法与_.sortBy类似，只是它允许指定要排序的迭代的排序顺序。 如果订单未指定，则所有值均按升序排序。 否则，请指定“desc”的降序顺序或“asc”顺序的升序顺序。

##### 初始

4.0.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratees=[_.identity]]` *（Array [] | Function [] | Object [] | string []）*：要进行排序的迭代。
3. `[orders]` *（string []）*：的排序顺序`iteratees`。

##### 返回

*（Array）*：返回新的排序数组。

##### 例

```javascript
var users = [
  { 'user': 'fred',   'age': 48 },

  { 'user': 'barney', 'age': 34 },

  { 'user': 'fred',   'age': 40 },

  { 'user': 'barney', 'age': 36 }
];
 
// Sort by `user` in ascending order and by `age` in descending order.
_.orderBy(users, ['user', 'age'], ['asc', 'desc']);
// => objects for [['barney', 36], ['barney', 34], ['fred', 48], ['fred', 40]]
```

### _.partition(collection, predicate=_.identity)

创建一个分成两组的元素数组，其中第一个包含谓词返回truthy for的元素，第二个包含predicate返回falsey的元素。 谓词用一个参数调用：（value）。

##### 初始

3.0.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回分组元素的数组。

##### 例

```javascript
var users = [
  { 'user': 'barney',  'age': 36, 'active': false },

  { 'user': 'fred',    'age': 40, 'active': true },

  { 'user': 'pebbles', 'age': 1,  'active': false }
];
 
_.partition(users, function(o) { return o.active; });
// => objects for [['fred'], ['barney', 'pebbles']]
 
// The `_.matches` iteratee shorthand.
_.partition(users, { 'age': 1, 'active': false });
// => objects for [['pebbles'], ['barney', 'fred']]
 
// The `_.matchesProperty` iteratee shorthand.
_.partition(users, ['active', false]);
// => objects for [['barney', 'pebbles'], ['fred']]
 
// The `_.property` iteratee shorthand.
_.partition(users, 'active');
// => objects for [['fred'], ['barney', 'pebbles']]
```

### _.reduce(collection, iteratee=_.identity, accumulator)

将集合减少到一个值，该值是通过迭代器运行collection中的每个元素的累积结果，其中每个连续的调用都提供了前一个的返回值。 如果没有累加器，则采集的第一个元素被用作初始值。 迭代器被调用四个参数：

*(accumulator, value, index|key, collection)*.

有许多lodash方法防护，iteratees的方法如`_.reduce`，`_.reduceRight`和`_.transform`。

防护的方法是：

```
assign`, `defaults`, `defaultsDeep`, `includes`, `merge`, `orderBy`, and `sortBy
```

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[accumulator]` *（\*）*：初始值。

##### 返回

*（\*）*：返回累计值。

##### 例

```javascript
_.reduce([1, 2], function(sum, n) {
  return sum + n;
}, 0);
// => 3
 
_.reduce({ 'a': 1, 'b': 2, 'c': 1 }, function(result, value, key) {
  (result[value] || (result[value] = [])).push(key);
  return result;
}, {});
// => { '1': ['a', 'c'], '2': ['b'] } (iteration order is not guaranteed)
```

### _.reduceRight(collection, iteratee=_.identity, accumulator)

这个方法就像`_.reduce`，不同的是它遍历`collection` 从右到左的元素。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[accumulator]` *（\*）*：初始值。

##### 返回

*（\*）*：返回累计值。

##### 例

```javascript
var array = [[0, 1], [2, 3], [4, 5]];
 
_.reduceRight(array, function(flattened, other) {
  return flattened.concat(other);
}, []);
// => [4, 5, 2, 3, 0, 1]
```

### _.reject(collection, predicate=_.identity)

与_.filter相反; 此方法返回谓词不返回真值的集合元素。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（数组）*：返回新的过滤数组。

##### 例

```javascript
var users = [
  { 'user': 'barney', 'age': 36, 'active': false },

  { 'user': 'fred',   'age': 40, 'active': true }
];
 
_.reject(users, function(o) { return !o.active; });
// => objects for ['fred']
 
// The `_.matches` iteratee shorthand.
_.reject(users, { 'age': 40, 'active': true });
// => objects for ['barney']
 
// The `_.matchesProperty` iteratee shorthand.
_.reject(users, ['active', false]);
// => objects for ['fred']
 
// The `_.property` iteratee shorthand.
_.reject(users, 'active');
// => objects for ['barney']
```

### _.sample(collection)

从`collection`中取一个随机元素。

##### 初始

2.0.0

##### 参数

1. `collection` *（Array | Object）*：要采样的集合。

##### 返回

*（\*）*：返回随机元素。

##### 例

```javascript
_.sample([1, 2, 3, 4]);
// => 2
```

### _.sampleSize(collection, n=1)

获取集合中唯一键的n个随机元素（大小不等）

##### 初始

4.0.0

##### 参数

1. `collection` *（Array | Object）*：要采样的集合。
2. `[n=1]` *（数字）*：要采样的元素数量。

##### 返回

*（数组）*：返回随机元素。

##### 例

```javascript
_.sampleSize([1, 2, 3], 2);
// => [3, 1]
 
_.sampleSize([1, 2, 3], 4);
// => [2, 3, 1]
```

### _.shuffle(collection)

使用[Fisher-Yates shuffle](https://en.wikipedia.org/wiki/Fisher-Yates_shuffle)版本创建一个混合值数组。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：要刷新的集合。

##### 返回

*（数组）*：返回新的混洗数组。

##### 例

```javascript
_.shuffle([1, 2, 3, 4]);
// => [4, 1, 3, 2]
```

### _.size(collection)

通过返回对象的数组类型值的长度或自己的可枚举字符串键控属性的数量来获取集合的大小。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object | string）*：要检查的集合。

##### 返回

*（数字）*：返回集合大小。

##### 例

```javascript
_.size([1, 2, 3]);
// => 3
 
_.size({ 'a': 1, 'b': 2 });
// => 2
 
_.size('pebbles');
// => 7
```

### _.some(collection, predicate=_.identity)

检查`predicate`是否返回**任何**`collection`的元素。 一旦谓词返回真，迭代就会停止。 谓词用三个参数调用：（value，index | key，collection）。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（boolean）*：若有元素通过谓词检查，返回`true，`否则`false`。

##### 例

```javascript
_.some([null, 0, 'yes', false], Boolean);
// => true
 

var users = [
  { 'user': 'barney', 'active': true },

  { 'user': 'fred',   'active': false }
];
 
// The `_.matches` iteratee shorthand.
_.some(users, { 'user': 'barney', 'active': false });
// => false
 
// The `_.matchesProperty` iteratee shorthand.
_.some(users, ['active', false]);
// => true
 
// The `_.property` iteratee shorthand.
_.some(users, 'active');
// => true
```

### _.sortBy(collection, [iteratees=_.identity])

创建一个元素数组，按照每个迭代器运行集合中每个元素的结果按升序排序。此方法执行稳定的排序，即保留相同元素的原始排序顺序。迭代被调用一个参数：*（value）*。

##### 初始

0.1.0

##### 参数

1. `collection` *（Array | Object）*：迭代的集合。
2. `[iteratees=[_.identity]]` *（...（Function | Function []））*：按迭代排序。

##### 返回

*（Array）*：返回新的排序数组。

##### 例

```javascript
var users = [
  { 'user': 'fred',   'age': 48 },

  { 'user': 'barney', 'age': 36 },

  { 'user': 'fred',   'age': 40 },

  { 'user': 'barney', 'age': 34 }
];
 
_.sortBy(users, [function(o) { return o.user; }]);
// => objects for [['barney', 36], ['barney', 34], ['fred', 48], ['fred', 40]]
 
_.sortBy(users, ['user', 'age']);
// => objects for [['barney', 34], ['barney', 36], ['fred', 40], ['fred', 48]]
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07