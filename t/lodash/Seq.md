# Seq

### _(value)

创建一个`lodash`包装`value`以启用隐式方法链序列的对象。操作和返回数组，集合和函数的方法可以链接在一起。检索单个值或可能返回原始值的方法将自动结束链序列并返回展开的值。否则，该值必须用解包`_#value`。

明确的链序列，其必须用包装除去 `_#value`，可以使用被使能`_.chain`。

链式方法的执行是懒惰的，也就是说，它被推迟到`_#value`隐式或显式调用。

懒惰评估允许几种方法来支持快捷融合。快捷融合是优化合并迭代调用; 这可以避免创建中间数组，并且可以大大减少迭代执行次数。如果该节应用于数组并且迭代仅接受一个参数，则链序列的节可以用于快捷融合。是否有资格进行快捷方式融合的启发式可能会发生变化。

只要`_#value`方法直接或间接包含在构建中，链接就支持在自定义构建中。

除了lodash方法，包装器`Array`和`String`方法。

包装`Array`方法是：

```
concat`, `join`, `pop`, `push`, `shift`, `sort`, `splice`, and `unshift
```

包装`String`方法是：

```
replace` 和`split
```

支持快捷融合的封装方法是：

```
at`, `compact`, `drop`, `dropRight`, `dropWhile`, `filter`, `find`, `findLast`, `head`, `initial`, `last`, `map`, `reject`, `reverse`, `slice`, `tail`, `take`, `takeRight`, `takeRightWhile`, `takeWhile`, and `toArray
```

The chainable wrapper methods are:

```
after`, `ary`, `assign`, `assignIn`, `assignInWith`, `assignWith`, `at`, `before`, `bind`, `bindAll`, `bindKey`, `castArray`, `chain`, `chunk`, `commit`, `compact`, `concat`, `conforms`, `constant`, `countBy`, `create`, `curry`, `debounce`, `defaults`, `defaultsDeep`, `defer`, `delay`, `difference`, `differenceBy`, `differenceWith`, `drop`, `dropRight`, `dropRightWhile`, `dropWhile`, `extend`, `extendWith`, `fill`, `filter`, `flatMap`, `flatMapDeep`, `flatMapDepth`, `flatten`, `flattenDeep`, `flattenDepth`, `flip`, `flow`, `flowRight`, `fromPairs`, `functions`, `functionsIn`, `groupBy`, `initial`, `intersection`, `intersectionBy`, `intersectionWith`, `invert`, `invertBy`, `invokeMap`, `iteratee`, `keyBy`, `keys`, `keysIn`, `map`, `mapKeys`, `mapValues`, `matches`, `matchesProperty`, `memoize`, `merge`, `mergeWith`, `method`, `methodOf`, `mixin`, `negate`, `nthArg`, `omit`, `omitBy`, `once`, `orderBy`, `over`, `overArgs`, `overEvery`, `overSome`, `partial`, `partialRight`, `partition`, `pick`, `pickBy`, `plant`, `property`, `propertyOf`, `pull`, `pullAll`, `pullAllBy`, `pullAllWith`, `pullAt`, `push`, `range`, `rangeRight`, `rearg`, `reject`, `remove`, `rest`, `reverse`, `sampleSize`, `set`, `setWith`, `shuffle`, `slice`, `sort`, `sortBy`, `splice`, `spread`, `tail`, `take`, `takeRight`, `takeRightWhile`, `takeWhile`, `tap`, `throttle`, `thru`, `toArray`, `toPairs`, `toPairsIn`, `toPath`, `toPlainObject`, `transform`, `unary`, `union`, `unionBy`, `unionWith`, `uniq`, `uniqBy`, `uniqWith`, `unset`, `unshift`, `unzip`, `unzipWith`, `update`, `updateWith`, `values`, `valuesIn`, `without`, `wrap`, `xor`, `xorBy`, `xorWith`, `zip`, `zipObject`, `zipObjectDeep`, and `zipWith
```

默认情况下**不可**链接的包装器方法是：

```
add`, `attempt`, `camelCase`, `capitalize`, `ceil`, `clamp`, `clone`, `cloneDeep`, `cloneDeepWith`, `cloneWith`, `conformsTo`, `deburr`, `defaultTo`, `divide`, `each`, `eachRight`, `endsWith`, `eq`, `escape`, `escapeRegExp`, `every`, `find`, `findIndex`, `findKey`, `findLast`, `findLastIndex`, `findLastKey`, `first`, `floor`, `forEach`, `forEachRight`, `forIn`, `forInRight`, `forOwn`, `forOwnRight`, `get`, `gt`, `gte`, `has`, `hasIn`, `head`, `identity`, `includes`, `indexOf`, `inRange`, `invoke`, `isArguments`, `isArray`, `isArrayBuffer`, `isArrayLike`, `isArrayLikeObject`, `isBoolean`, `isBuffer`, `isDate`, `isElement`, `isEmpty`, `isEqual`, `isEqualWith`, `isError`, `isFinite`, `isFunction`, `isInteger`, `isLength`, `isMap`, `isMatch`, `isMatchWith`, `isNaN`, `isNative`, `isNil`, `isNull`, `isNumber`, `isObject`, `isObjectLike`, `isPlainObject`, `isRegExp`, `isSafeInteger`, `isSet`, `isString`, `isUndefined`, `isTypedArray`, `isWeakMap`, `isWeakSet`, `join`, `kebabCase`, `last`, `lastIndexOf`, `lowerCase`, `lowerFirst`, `lt`, `lte`, `max`, `maxBy`, `mean`, `meanBy`, `min`, `minBy`, `multiply`, `noConflict`, `noop`, `now`, `nth`, `pad`, `padEnd`, `padStart`, `parseInt`, `pop`, `random`, `reduce`, `reduceRight`, `repeat`, `result`, `round`, `runInContext`, `sample`, `shift`, `size`, `snakeCase`, `some`, `sortedIndex`, `sortedIndexBy`, `sortedLastIndex`, `sortedLastIndexBy`, `startCase`, `startsWith`, `stubArray`, `stubFalse`, `stubObject`, `stubString`, `stubTrue`, `subtract`, `sum`, `sumBy`, `template`, `times`, `toFinite`, `toInteger`, `toJSON`, `toLength`, `toLower`, `toNumber`, `toSafeInteger`, `toString`, `toUpper`, `trim`, `trimEnd`, `trimStart`, `truncate`, `unescape`, `uniqueId`, `upperCase`, `upperFirst`, `value`, and `words
```

##### 参数

1. `value` *（\*）*：要包装在`lodash`实例中的值。

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
function square(n) {
  return n * n;
}
 

var wrapped = _([1, 2, 3]);
 
// Returns an unwrapped value.
wrapped.reduce(_.add);
// => 6
 
// Returns a wrapped value.

var squares = wrapped.map(square);
 
_.isArray(squares);
// => false
 
_.isArray(squares.value());
// => true
```

### _.chain(value)

创建一个 `lodash`封装器实例，该实例包含`value`启用显式方法链序列的封装。这些序列的结果必须用包装来解开`_#value`。

##### 版本

1.3.0

##### 参数

1. `value` *（\*）*：要包装的值。

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
var users = [
  { 'user': 'barney',  'age': 36 },

  { 'user': 'fred',    'age': 40 },

  { 'user': 'pebbles', 'age': 1 }
];
 

var youngest = _
  .chain(users)
  .sortBy('age')
  .map(function(o) {
    return o.user + ' is ' + o.age;
  })
  .head()
  .value();
// => 'pebbles is 1'
```

### _.tap(value, interceptor)

此方法调用`interceptor`并返回`value`。拦截器被调用一个参数; *（价值）*。这种方法的目的是“挖掘”一个方法链序列以修改中间结果。

##### 版本

0.1.0

##### 参数

1. `value` *（\*）*：提供给的值`interceptor`。
2. `interceptor` *（功能）*：调用的功能。

##### 返回

*（\*）*：返回`value`。

##### 例

```javascript
_([1, 2, 3])
 .tap(function(array) {

// Mutate input array.

   array.pop();
 })
 .reverse()
 .value();
// => [2, 1]
```

### _.thru(value, interceptor)

##### 版本

3.0.0

##### 参数

1. `value` *（\*）*：提供给的值`interceptor`。
2. `interceptor` *（功能）*：调用的功能。

##### 返回

*（\*）*：返回结果`interceptor`。

##### 例

```javascript
_('  abc  ')
 .chain()
 .trim()
 .thru(function(value) {
   return [value];
 })
 .value();
// => ['abc']
```

### _.prototypeSymbol.iterator

使包装可以迭代。

##### 版本

4.0.0

##### 返回

*（Object）*：返回包装器对象。

##### 例

```javascript
var wrapped = _([1, 2]);
 
wrapped[Symbol.iterator]() === wrapped;
// => true
 

Array.from(wrapped);
// => [1, 2]
```

### _.prototype.at(paths)

##### 版本

1.0.0

##### 参数

1. `[paths]` *（...（string | string []））*：要选择的属性路径。

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 3 } }, 4] };
 

_(object).at(['a[0].b.c', 'a[1]']).value();
// => [3, 4]
```

### _.prototype.chain()

`lodash`使用显式的方法链序列创建一个包装器实例。

##### 版本

0.1.0

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
var users = [
  { 'user': 'barney', 'age': 36 },

  { 'user': 'fred',   'age': 40 }
];
 
// A sequence without explicit chaining.

_(users).head();
// => { 'user': 'barney', 'age': 36 }
 
// A sequence with explicit chaining.

_(users)
  .chain()
  .head()
  .pick('user')
  .value();
// => { 'user': 'barney' }
```

### _.prototype.commit()

执行链顺序并返回包装结果。

##### 版本

3.2.0

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
var array = [1, 2];

var wrapped = _(array).push(3);
 

console.log(array);
// => [1, 2]
 
wrapped = wrapped.commit();

console.log(array);
// => [1, 2, 3]
 
wrapped.last();
// => 3
 

console.log(array);
// => [1, 2, 3]
```

### _.prototype.next()

##### 版本

4.0.0

##### 返回

*（Object）*：返回下一个迭代器值。

##### 例

```javascript
var wrapped = _([1, 2]);
 
wrapped.next();
// => { 'done': false, 'value': 1 }
 
wrapped.next();
// => { 'done': false, 'value': 2 }
 
wrapped.next();
// => { 'done': true, 'value': undefined }
```

### _.prototype.plant(value)

创建链序列种植的克隆`value`作为包装值。

##### 版本

3.2.0

##### 参数

1. `value` *（\*）*：plant的取值。

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### Example

```javascript
function square(n) {
  return n * n;
}
 

var wrapped = _([1, 2]).map(square);

var other = wrapped.plant([3, 4]);
 
other.value();
// => [9, 16]
 
wrapped.value();
// => [1, 4]
```

### _.prototype.reverse()

##### Since

0.1.0

##### 返回

*（Object）*：返回新的`lodash`包装器实例。

##### 例

```javascript
var array = [1, 2, 3];
 

_(array).reverse().value()
// => [3, 2, 1]
 

console.log(array);
// => [3, 2, 1]
```

### _.prototype.value()

执行链序列来解析展开的值。

##### 版本

0.1.0

##### 别名

*_.prototype.toJSON, _.prototype.valueOf*

##### 返回

*（\*）*：返回已解析的解包值。

##### 例

```javascript
_([1, 2, 3]).value();
// => [1, 2, 3]
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07