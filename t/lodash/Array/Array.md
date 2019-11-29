# Array

- 贡献者1人

  

### _.chunk(array, size=1)

创建一个元素数组，分成长度为`size`。如果`array`不能均匀分割，最后的块将成为剩余的元素。

##### 初始值

3.0.0

##### 参数

1. `array` *（Array）*：要处理的数组。
2. `[size=1]` *（大小）*：每个块的长度

##### 返回值

*（Array）*：返回新的数组块。

##### 示例

```javascript
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]
 
_.chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]
```

### _.compact(array)

创建一个数组，其中删除所有falsey值。以下的值——`false`，`null`，`0`，`""`，`undefined`，和`NaN`是 falsey。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要压缩的数组。

##### 返回值

*（Array）*：返回新的过滤值数组。

##### 示例

```javascript
_.compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```

### _.concat(array, values)

创建`array`与任何其他数组和/或值连接的新数组。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要连接的数组。
2. `[values]` *（... \*）*：要连接的值。

##### 返回值

*（Array）*：返回新的连接数组。

##### 示例

```javascript
var array = [1];

var other = _.concat(array, 2, [3], [[4]]);
 

console.log(other);
// => [1, 2, 3, [4]]
 

console.log(array);
// => [1]
```

### _.difference(array, values)

`array`使用[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)相等性比较创建未包含在其他给定数组中的值数组。结果值的顺序和引用由第一个数组确定。

**注意：**与`_.pullAll`此不同，此方法返回一个新数组。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[values]` *（... Array）*：要排除的值。

##### 返回值

*（Array）*：返回新的过滤值数组。

##### 示例

```javascript
_.difference([2, 1], [2, 3]);
// => [1]
```

### _.differenceBy(array, values, iteratee=_.identity)

这种方法类似于`_.difference`，不同的是它接受`iteratee` 并为每个元素调用`array`并`values`生成它们进行比较的标准。结果值的顺序和引用由第一个数组确定。迭代器因此调用一个参数：

*(value)*.

**注意：**与`_.pullAllBy`此不同，此方法返回一个新数组。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[values]` *（... Array）*：要排除的值。
3. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回值

*（Array）*：返回新的过滤值数组。

##### 示例

```javascript
_.differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor);
// => [1.2]
 
// The `_.property` iteratee shorthand.
_.differenceBy([{ 'x': 2 }, { 'x': 1 }], [{ 'x': 1 }], 'x');
// => [{ 'x': 2 }]
```

### _.differenceWith(array, values, comparator)

这种方法与`_.difference`类似，不同之处在于它接受 `comparator`被调用来比较的元素`array`到`values`。结果值的顺序和引用由第一个数组确定。比较器因此调用两个参数：*（arrVal，othVal）*。

**注意：**与`_.pullAllWith`此不同，此方法返回一个新数组。

##### 初始

4.0.0

##### 参数

1. `array` *(Array)*: The array to inspect.
2. `[values]` *(...Array)*: The values to exclude.
3. `[comparator]` *(Function)*: The comparator invoked per element.

##### 返回

*（Array）*：返回新的过滤值数组。

##### 示例

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
 
_.differenceWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual);
// => [{ 'x': 2, 'y': 1 }]
```

### _.drop(array, n=1)

创建`n`元素从一开始就下降的部分`array`

##### 初始

0.5.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[n=1]` *（数字）*：要删除的元素数量。

##### 返回

*（数组）*：返回部分`array`。

##### 示例

```javascript
_.drop([1, 2, 3]);
// => [2, 3]
 
_.drop([1, 2, 3], 2);
// => [3]
 
_.drop([1, 2, 3], 5);
// => []
 
_.drop([1, 2, 3], 0);
// => [1, 2, 3]
```

### _.dropRight(array, n=1)

创建从最后开始使用`n`删除元素的一部分`array`

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[n=1]` *（数字）*：要删除的元素数量。

##### 返回

*（数组）*：返回一部分`array`。

##### 示例

```javascript
_.dropRight([1, 2, 3]);
// => [1, 2]
 
_.dropRight([1, 2, 3], 2);
// => [1]
 
_.dropRight([1, 2, 3], 5);
// => []
 
_.dropRight([1, 2, 3], 0);
// => [1, 2, 3]
```

### _.dropRightWhile(array, predicate=_.identity)

创建一个`array`从结尾删除的排除元素的片段。元素被丢弃，直到`predicate`返回falsey。谓词用三个参数调用：*（value，index，array）*。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（数组）*：返回一部分`array`。

##### 示例

```javascript
var users = [
  { 'user': 'barney',  'active': true },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': false }
];
 
_.dropRightWhile(users, function(o) { return !o.active; });
// => objects for ['barney']
 
// The `_.matches` iteratee shorthand.
_.dropRightWhile(users, { 'user': 'pebbles', 'active': false });
// => objects for ['barney', 'fred']
 
// The `_.matchesProperty` iteratee shorthand.
_.dropRightWhile(users, ['active', false]);
// => objects for ['barney']
 
// The `_.property` iteratee shorthand.
_.dropRightWhile(users, 'active');
// => objects for ['barney', 'fred', 'pebbles']
```

### _.dropWhile(array, predicate=_.identity)

创建从初始删除的排除元素的一部分`array`。元素被丢弃，直到 `predicate`返回falsey。谓词用三个参数调用：*（value，index，array）*。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（数组）*：返回部分`array`。

##### 示例

```javascript
var users = [
  { 'user': 'barney',  'active': false },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': true }
];
 
_.dropWhile(users, function(o) { return !o.active; });
// => objects for ['pebbles']
 
// The `_.matches` iteratee shorthand.
_.dropWhile(users, { 'user': 'barney', 'active': false });
// => objects for ['fred', 'pebbles']
 
// The `_.matchesProperty` iteratee shorthand.
_.dropWhile(users, ['active', false]);
// => objects for ['pebbles']
 
// The `_.property` iteratee shorthand.
_.dropWhile(users, 'active');
// => objects for ['barney', 'fred', 'pebbles']
```

### _.fill(array, value, start=0, end=array.length)

填充的元素`array`与`value`来自`start`为止，但不包括`end`。

**注意：** 此方法发生变化`array`。

##### 初始

3.2.0

##### 参数

1. `array` *（Array）*：要填充的数组。
2. `value` *（\*）*：填写的值`array`。
3. `[start=0]` *（* *number* *）*：开始位置。
4. `[end=array.length]` *（* *number* *）*：结束位置。

##### 返回

*（Array）*：返回`array`。

##### 示例

```javascript
var array = [1, 2, 3];
 
_.fill(array, 'a');

console.log(array);
// => ['a', 'a', 'a']
 
_.fill(Array(3), 2);
// => [2, 2, 2]
 
_.fill([4, 6, 8, 10], '*', 1, 3);
// => [4, '*', '*', 10]
```

### _.findIndex(array, predicate=_.identity, fromIndex=0)

这个方法就像`_.find`，不同的是它返回第一个元素的索引，`predicate`返回truthy而不是元素本身。

##### 初始

1.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[fromIndex=0]` *（number）*：从中搜索的索引。

##### 返回

*（number）*：返回找到的元素的索引，else `-1`。

##### 示例

```javascript
var users = [
  { 'user': 'barney',  'active': false },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': true }
];
 
_.findIndex(users, function(o) { return o.user == 'barney'; });
// => 0
 
// The `_.matches` iteratee shorthand.
_.findIndex(users, { 'user': 'fred', 'active': false });
// => 1
 
// The `_.matchesProperty` iteratee shorthand.
_.findIndex(users, ['active', false]);
// => 0
 
// The `_.property` iteratee shorthand.
_.findIndex(users, 'active');
// => 2
```

### _.findLastIndex(array, predicate=_.identity, fromIndex=array.length-1)

这个方法就像`_.findIndex`，不同的是它遍历`collection` 从右到左的元素。

##### 初始

2.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[fromIndex=array.length-1]` *（number）*：从中搜索的索引。

##### 返回

*（number）*：返回找到的元素的索引，否则返回`-1`。

##### 示例

```javascript
var users = [
  { 'user': 'barney',  'active': true },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': false }
];
 
_.findLastIndex(users, function(o) { return o.user == 'pebbles'; });
// => 2
 
// The `_.matches` iteratee shorthand.
_.findLastIndex(users, { 'user': 'barney', 'active': true });
// => 0
 
// The `_.matchesProperty` iteratee shorthand.
_.findLastIndex(users, ['active', false]);
// => 2
 
// The `_.property` iteratee shorthand.
_.findLastIndex(users, 'active');
// => 0
```

### _.flatten(array)

以单一的深度统一array

##### 初始

0.1.0

##### 参数

1. `array` *（数组）*：要平化的数组。

##### 返回

*（* *Array* *）*：返回新的展平数组。

##### 示例

```javascript
_.flatten([1, [2, [3, [4]], 5]]);
// => [1, 2, [3, [4]], 5]
```

### _.flattenDeep(array)

递归地平化`array`。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要平化的数组。

##### 返回

*（Array）*：返回新的展平数组。

##### 示例

```javascript
_.flattenDeep([1, [2, [3, [4]], 5]]);
// => [1, 2, 3, 4, 5]
```

### _.flattenDepth(array, depth=1)

递归平化`array`高达`depth`倍。

##### 初始

4.4.0

##### 参数

1. `array` *（Array）*：要平化的数组。
2. `[depth=1]` *（number）*：最大递归深度。

##### 返回

*(Array)*: 返回新的平化数组

##### 示例

```javascript
var array = [1, [2, [3, [4]], 5]];
 
_.flattenDepth(array, 1);
// => [1, 2, [3, [4]], 5]
 
_.flattenDepth(array, 2);
// => [1, 2, 3, [4], 5]
```

### _.fromPairs(pairs)

相反的`_.toPairs`; 此方法返回由键值组成的对象 `pairs`。

##### 初始

4.0.0

##### 参数

1. `pairs` *(Array)*: 值键对

##### 返回

*（Object）*：返回新的对象。

##### 示例

```javascript
_.fromPairs([['a', 1], ['b', 2]]);
// => { 'a': 1, 'b': 2 }
```

### _.head(array)

获取`array`的第一个元素。

##### 初始

0.1.0

##### 别名

*_.first*

##### 参数

1. `array` *（Array）*：要查询的数组。

##### 返回

*（\*）*：返回`array`的第一个元素。

##### 示例

```javascript
_.head([1, 2, 3]);
// => 1
 
_.head([]);
// => undefined
```

### _.indexOf(array, value, fromIndex=0)

获取在其中第一次出现的索引，与`value`中被发现`array`使用[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)的相等比较。如果`fromIndex`为负值，则将其用作从结尾开始的偏移量`array`。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `value` *（\*）*：要搜索的值。
3. `[fromIndex=0]` *（number）*：从中搜索的索引。

##### 返回

*（number）*：返回匹配值的索引，否则返回`-1`。

##### 示例

```javascript
_.indexOf([1, 2, 1, 2], 2);
// => 1
 
// Search from the `fromIndex`.
_.indexOf([1, 2, 1, 2], 2, 2);
// => 3
```

### _.initial(array)

获得`array`中除了最后一个元素之外的所有元素

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要查询的数组。

##### 返回

*（Array）*：返回部分`array`。

##### 示例

```javascript
_.initial([1, 2, 3]);
// => [1, 2]
```

### _.intersection(arrays)

创建一个包含在所有给定数组中的唯一值数组，[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)用于相等性比较。结果值的顺序和引用由第一个数组确定。

##### 初始

0.1.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。

##### 返回

*（Array）*：返回相交值的新数组。

##### 示例

```javascript
_.intersection([2, 1], [2, 3]);
// => [2]
```

### _.intersectionBy(arrays, iteratee=_.identity)

这种方法类似于`_.intersection`，不同的是它接受 `iteratee`为每个元素的每个元素调用`arrays`以生成它们进行比较的标准。结果值的顺序和引用由第一个数组确定。迭代器因此调用一个参数：

*(value)*.

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回相交值的新数组。

##### 示例

```javascript
_.intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor);
// => [2.1]
 
// The `_.property` iteratee shorthand.
_.intersectionBy([{ 'x': 1 }], [{ 'x': 2 }, { 'x': 1 }], 'x');
// => [{ 'x': 1 }]
```

### _.intersectionWith(arrays, comparator)

这个方法类似于`_.intersection`不同的是它接受`comparator`哪个被调用来比较元素`arrays`。结果值的顺序和引用由第一个数组确定。比较器调用两个参数：*（arrVal，othVal）*。

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[comparator]` *（Function）*：每个元素调用比较器。

##### 返回

*（Array）*：返回相交值的新数组。

##### 示例

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];

var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
 
_.intersectionWith(objects, others, _.isEqual);
// => [{ 'x': 1, 'y': 2 }]
```

### _.join(array, separator=',')

将`array`中所有元素转换为由separator分隔的字符串

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要转换的数组。
2. `[separator=',']` *（string）*：元素分隔符。

##### 返回

*（string）*：返回连接的字符串。

##### 示例

```javascript
_.join(['a', 'b', 'c'], '~');
// => 'a~b~c'
```

### _.last(array)

获取`array`最后一个元素。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要查询的数组。

##### 返回

*（\*）*：返回`array`的最后一个元素。

##### 示例

```javascript
_.last([1, 2, 3]);
// => 3
```

### _.lastIndexOf(array, value, fromIndex=array.length-1)

这个方法就像`_.indexOf`，不同的是它遍历`array` 从右到左的元素。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `value` *（\*）*：要搜索的值。
3. `[fromIndex=array.length-1]` *（number）*：从中搜索的索引。

##### 返回

*（number）*：返回匹配值的索引，否则返回`-1`。

##### 示例

```javascript
_.lastIndexOf([1, 2, 1, 2], 2);
// => 3
 
// Search from the `fromIndex`.
_.lastIndexOf([1, 2, 1, 2], 2, 2);
// => 1
```

### _.nth(array, n=0)

获取索引`n`处的元素`array`。如果`n`是负数，则返回从结尾开始的第n个元素。

##### 初始

4.11.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[n=0]` *（number）*：要返回的元素的索引。

##### 返回

*（\*）*：返回`array`的第n个元素。

##### 示例

```javascript
var array = ['a', 'b', 'c', 'd'];
 
_.nth(array, 1);
// => 'b'
 
_.nth(array, -2);
// => 'c';
```

### _.pull(array, values)

将删除所有给定值`array`用[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)平等地比较。

**注意：**与`_.without`不同，此方法会发生使`array`变化，用`_.remove`谓词从数组中移除元素。

##### 初始

2.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `[values]` *（... \*）*：要删除的值。

##### 返回

*（Array）*：返回`array`。

##### 示例

```javascript
var array = ['a', 'b', 'c', 'a', 'b', 'c'];
 
_.pull(array, 'a', 'c');

console.log(array);
// => ['b', 'b']
```

### _.pullAll(array, values)

这个方法就像`_.pull`，不同的是它接受一个要移除的值的数组。

**注意：**与`_.difference`此不同，此方法会使`array`发生变化。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `values` *（Array）*：要删除的值。

##### 返回

*（Array）*：返回`array`。

##### 示例

```javascript
var array = ['a', 'b', 'c', 'a', 'b', 'c'];
 
_.pullAll(array, ['a', 'c']);

console.log(array);
// => ['b', 'b']
```

### _.pullAllBy(array, values, iteratee=_.identity)

这种方法类似于`_.pullAll`，不同的是它接受`iteratee`为每个元素调用`array`并`values`生成它们进行比较的标准。迭代器调用一个参数：*（value）*。

**注意：**与`_.differenceBy`此不同，此方法会使`array`发生变化。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `values` *（Array）*：要删除的值。
3. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回`array`。

##### 例

```javascript
var array = [{ 'x': 1 }, { 'x': 2 }, { 'x': 3 }, { 'x': 1 }];
 
_.pullAllBy(array, [{ 'x': 1 }, { 'x': 3 }], 'x');

console.log(array);
// => [{ 'x': 2 }]
```

### _.pullAllWith(array, values, comparator)

这种方法如`_.pullAll`，不同之处在于它接受`comparator` 被调用来比较的元素`array`到`values`。比较器被调用两个参数：*（arrVal，othVal）*。

**注意：**与`_.differenceWith`不同，此方法会使`array`发生变化。

##### 初始

4.6.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `values` *（Array）*：要删除的值。
3. `[comparator]` *（Function）*：每个元素调用比较器。

##### 返回

*（Array）*：返回`array`。

##### 示例

```javascript
var array = [{ 'x': 1, 'y': 2 }, { 'x': 3, 'y': 4 }, { 'x': 5, 'y': 6 }];
 
_.pullAllWith(array, [{ 'x': 3, 'y': 4 }], _.isEqual);

console.log(array);
// => [{ 'x': 1, 'y': 2 }, { 'x': 5, 'y': 6 }]
```

### _.pullAt(array, indexes)

从`array`相应的元素中移除元素`indexes`并返回一个已移除元素的数组。

**注意：**与`_.at`此不同，此方法会使`array`发生变化 。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `[indexes]` *（...（number | number []））*：要移除的元素的索引。

##### 返回

*（Array）*：返回已移除元素的新数组。

##### 例

```javascript
var array = ['a', 'b', 'c', 'd'];

var pulled = _.pullAt(array, [1, 3]);
 

console.log(array);
// => ['a', 'c']
 

console.log(pulled);
// => ['b', 'd']
```

### _.remove(array, predicate=_.identity)

从`array`移除所有元素，`predicate`返回truthy并返回删除的元素的阵列。谓词用三个参数调用：*（value，index，array）*。

**注意：** 与`_.filter`此不同，此方法会使之发生变化。用于`_.pull`通过值从数组中提取元素。

##### 初始

2.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回已移除元素的新数组。

##### 例

```javascript
var array = [1, 2, 3, 4];

var evens = _.remove(array, function(n) {
  return n % 2 == 0;
});
 

console.log(array);
// => [1, 3]
 

console.log(evens);
// => [2, 4]
```

### _.reverse(array)

反转`array`使第一个元素成为最后一个，第二个元素成为倒数第二个元素，依此类推。

**注意：**此方法变异于`array`并基于[`Array#reverse`](https://mdn.io/Array/reverse)。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要修改的数组。

##### 返回

*（数组）*：返回`array`。

##### 例

```javascript
var array = [1, 2, 3];
 
_.reverse(array);
// => [3, 2, 1]
 

console.log(array);
// => [3, 2, 1]
```

### _.slice(array, start=0, end=array.length)

创建一个`array`从`start`一直到但不包括自身的部分`end`。

**注意：**此方法用于[`Array#slice`](https://mdn.io/Array/slice) 以确保返回密集数组。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要切片的数组。
2. `[start=0]` *（number）*：开始位置。
3. `[end=array.length]` *（number）*：结束位置。

##### 返回

*（Array）*：返回部分`array`。

### _.sortedIndex(array, value)

使用二进制搜索来确定`value`应该插入的最低索引即`array`的最低索引以便维护其排序顺序。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的排序数组。
2. `value` *（\*）*：要评估的值。

##### 返回

*（number）*：返回`value`应插入的索引`array`。

##### 例

```javascript
_.sortedIndex([30, 50], 40);
// => 1
```

### _.sortedIndexBy(array, value, iteratee=_.identity)

这个方法就像`_.sortedIndex`,不同的是它接受 `iteratee`调用的方法`value`和每个元素`array`来计算它们的排序顺序。迭代器被调用一个参数：*（value）*。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的排序数组。
2. `value` *（\*）*：要评估的值。
3. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（number）*：返回`value`应插入的索引`array`。

##### 例

```javascript
var objects = [{ 'x': 4 }, { 'x': 5 }];
 
_.sortedIndexBy(objects, { 'x': 4 }, function(o) { return o.x; });
// => 0
 
// The `_.property` iteratee shorthand.
_.sortedIndexBy(objects, { 'x': 4 }, 'x');
// => 0
```

### _.sortedIndexOf(array, value)

这种方法就像`_.indexOf`，不同的是它在排序后执行二分搜索 `array`。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `value` *（\*）*：要搜索的值。

##### 返回

*（number）*：返回匹配值的索引，否则返回`-1`。

##### 例

```javascript
_.sortedIndexOf([4, 5, 5, 5, 6], 5);
// => 1
```

### _.sortedLastIndex(array, value)

这个方法就像`_.sortedIndex`，不同的是它返回`value`应该插入`array`的最高索引，以便维护它的排序顺序。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要检查的排序数组。
2. `value` *（\*）*：要评估的值。

##### 返回

*（number）*：返回`value`应插入`array`的索引。

##### 例

```javascript
_.sortedLastIndex([4, 5, 5, 5, 6], 5);
// => 4
```

### _.sortedLastIndexBy(array, value, iteratee=_.identity)

这个方法就像`_.sortedLastIndex`，不同的是它接受`iteratee` 调用的方法`value`和每个元素`array`来计算它们的排序顺序。迭代器被调用一个参数：*（value）*。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的排序数组。
2. `value` *（\*）*：要评估的值。
3. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（number）*：返回`value`应插入的索引`array`。

##### 例

```javascript
var objects = [{ 'x': 4 }, { 'x': 5 }];
 
_.sortedLastIndexBy(objects, { 'x': 4 }, function(o) { return o.x; });
// => 1
 
// The `_.property` iteratee shorthand.
_.sortedLastIndexBy(objects, { 'x': 4 }, 'x');
// => 1
```

### _.sortedLastIndexOf(array, value)

这种方法就像`_.lastIndexOf`，不同的是它在排序后执行二分搜索`array`。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `value` *（\*）*：要搜索的值。

##### 返回

*（number）*：返回匹配值的索引，否则返回`-1`。

##### 例

```javascript
_.sortedLastIndexOf([4, 5, 5, 5, 6], 5);
// => 3
```

### _.sortedUniq(array)

这种方法就像`_.uniq`，不同的是它为排序数组设计和优化的。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。

##### 返回

*（Array）*：返回新的重复空闲数组。

##### 例

```javascript
_.sortedUniq([1, 1, 2]);
// => [1, 2]
```

### _.sortedUniqBy(array, iteratee)

这种方法就像`_.uniqBy`，不同的是它是为排序数组设计和优化的。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[iteratee]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回新的重复空闲数组。

##### 例

```javascript
_.sortedUniqBy([1.1, 1.2, 2.3, 2.4], Math.floor);
// => [1.1, 2.3]
```

### _.tail(array)

除了第一个元素之外，都获得`array`的所有元素。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。

##### 返回

*（Array）*：返回部分`array`。

##### 例

```javascript
_.tail([1, 2, 3]);
// => [2, 3]
```

### _.take(array, n=1)

使用`n`从头开始的元素创建一部分`array`。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[n=1]` *（number）*：要采取的元素数量。

##### 返回

*（Array）*：返回部分`array`。

##### 例

```javascript
_.take([1, 2, 3]);
// => [1]
 
_.take([1, 2, 3], 2);
// => [1, 2]
 
_.take([1, 2, 3], 5);
// => [1, 2, 3]
 
_.take([1, 2, 3], 0);
// => []
```

### _.takeRight(array, n=1)

创建一个从最终获得的`n`元素的一部分`array`

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[n=1]` *（number）*：要采取的元素数量。

##### 返回

*（number）*：返回部分`array`。

##### 例

```javascript
_.takeRight([1, 2, 3]);
// => [3]
 
_.takeRight([1, 2, 3], 2);
// => [2, 3]
 
_.takeRight([1, 2, 3], 5);
// => [1, 2, 3]
 
_.takeRight([1, 2, 3], 0);
// => []
```

### _.takeRightWhile(array, predicate=_.identity)

创建一个从最后采取的元素的一部分`array`。提取元素，直到 `predicate`返回falsey为止。谓词用三个参数调用：*（value，index，array）*。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回部分`array`。

##### 例

```javascript
var users = [
  { 'user': 'barney',  'active': true },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': false }
];
 
_.takeRightWhile(users, function(o) { return !o.active; });
// => objects for ['fred', 'pebbles']
 
// The `_.matches` iteratee shorthand.
_.takeRightWhile(users, { 'user': 'pebbles', 'active': false });
// => objects for ['pebbles']
 
// The `_.matchesProperty` iteratee shorthand.
_.takeRightWhile(users, ['active', false]);
// => objects for ['fred', 'pebbles']
 
// The `_.property` iteratee shorthand.
_.takeRightWhile(users, 'active');
// => []
```

### _.takeWhile(array, predicate=_.identity)

使用从头开始的元素创建一部分`array`。提取元素直到`predicate` 返回false为止。谓词用三个参数调用：*（value，index，array）*。

##### 初始

3.0.0

##### 参数

1. `array` *（Array）*：要查询的数组。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Array）*：返回部分`array`。

##### 例

```javascript
var users = [
  { 'user': 'barney',  'active': false },

  { 'user': 'fred',    'active': false },

  { 'user': 'pebbles', 'active': true }
];
 
_.takeWhile(users, function(o) { return !o.active; });
// => objects for ['barney', 'fred']
 
// The `_.matches` iteratee shorthand.
_.takeWhile(users, { 'user': 'barney', 'active': false });
// => objects for ['barney']
 
// The `_.matchesProperty` iteratee shorthand.
_.takeWhile(users, ['active', false]);
// => objects for ['barney', 'fred']
 
// The `_.property` iteratee shorthand.
_.takeWhile(users, 'active');
// => []
```

### _.union(arrays)

从所有给定的数组中依次创建一个唯一值数组，[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)用于相等性比较。

##### 初始

0.1.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。

##### 返回

*（Array）*：返回组合值的新数组。

##### 例

```javascript
_.union([2], [1, 2]);
// => [2, 1]
```

### _.unionBy(arrays, iteratee=_.identity)

这种方法类似，`_.union`，不同之处在于它接受`iteratee`为每个元素调用每个元素`arrays`以生成唯一性计算标准。结果值从第一个出现值的数组中选择。迭代器被调用一个参数：

*(value)*.

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回组合值的新数组。

##### 例

```javascript
_.unionBy([2.1], [1.2, 2.3], Math.floor);
// => [2.1, 1.2]
 
// The `_.property` iteratee shorthand.
_.unionBy([{ 'x': 1 }], [{ 'x': 2 }, { 'x': 1 }], 'x');
// => [{ 'x': 1 }, { 'x': 2 }]
```

### _.unionWith(arrays, comparator)

这个方法类似于`_.union`，不同的是它接受`comparator`哪个被调用来比较元素`arrays`。结果值从第一个出现值的数组中选择。比较器被调用两个参数：*（arrVal，othVal）*。

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[comparator]` *（Function）*：每个元素调用比较器。

##### 返回

*（Array）*：返回组合值的新数组。

##### 例

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];

var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
 
_.unionWith(objects, others, _.isEqual);
// => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 1 }]
```

### _.uniq(array)

创建数组的无重复版本，[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)用于相等比较，其中只保留每个元素的第一个匹配项。结果值的顺序由它们在数组中出现的顺序决定。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。

##### 返回

*（Array）*：返回新的重复空闲数组。

##### 例

```javascript
_.uniq([2, 1, 2]);
// => [2, 1]
```

### _.uniqBy(array, iteratee=_.identity)

此方法类似于`_.uniq`，不同的是它接受`iteratee`为每个元素调用`array`以生成唯一性计算标准的方法。结果值的顺序由它们在数组中出现的顺序决定。迭代器被调用一个参数：

*(value)*.

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回新的重复空闲数组。

##### 例

```javascript
_.uniqBy([2.1, 1.2, 2.3], Math.floor);
// => [2.1, 1.2]
 
// The `_.property` iteratee shorthand.
_.uniqBy([{ 'x': 1 }, { 'x': 2 }, { 'x': 1 }], 'x');
// => [{ 'x': 1 }, { 'x': 2 }]
```

### _.uniqWith(array, comparator)

这个方法类似于`_.uniq`，不同的是它接受`comparator`哪个被调用来比较元素`array`。结果值的顺序由它们在数组中出现的顺序决定。使用两个参数调用比较器：*（arrVal，othVal）*。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[comparator]` *（Function）*：每个元素调用比较器。

##### 返回

*（Array）*：返回新的重复空闲数组。

##### 例

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 2 }];
 
_.uniqWith(objects, _.isEqual);
// => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }]
```

### _.unzip(array)

这种方法就像`_.zip`，不同的是它接受一个分组元素数组，并创建一个数组，将这些元素重新组合到它们的pre-zip配置中。

##### 初始

1.2.0

##### 参数

1. `array` *（Array）*：要处理的分组元素的数组。

##### 返回

*（Array）*：返回重新组合元素的新数组。

##### 例

```javascript
var zipped = _.zip(['a', 'b'], [1, 2], [true, false]);
// => [['a', 1, true], ['b', 2, false]]
 
_.unzip(zipped);
// => [['a', 'b'], [1, 2], [true, false]]
```

### _.unzipWith(array, iteratee=_.identity)

这种方法就像`_.unzip`，不同的是它接受`iteratee`指定应该如何组合重组值。迭代器用每个组的元素调用： *（... group）*。

##### 初始

3.8.0

##### 参数

1. `array` *（Array）*：要处理的分组元素的数组。
2. `[iteratee=_.identity]` *（function）*：组合重组值的功能。

##### 返回

*（Array）*：返回重新组合元素的新数组。

##### 例

```javascript
var zipped = _.zip([1, 2], [10, 20], [100, 200]);
// => [[1, 10, 100], [2, 20, 200]]
 
_.unzipWith(zipped, _.add);
// => [3, 30, 300]
```

### _.without(array, values)

使用[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero)相等比较创建排除所有给定值的数组。

**注意：**与`_.pull`不同，此方法返回一个新数组。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：要检查的数组。
2. `[values]` *（... \*）*：要排除的值。

##### 返回

*（Array）*：返回新的过滤值数组。

##### 例

```javascript
_.without([2, 1, 2, 3], 1, 2);
// => [3]
```

### _.xor（阵列）

创建一个唯一值的数组，它是给定数组的[对称差异](https://en.wikipedia.org/wiki/Symmetric_difference)。结果值的顺序由它们在数组中出现的顺序决定。

##### 初始

2.4.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。

##### 返回

*（Array）*：返回新的过滤值数组。

##### 例

```javascript
_.xor([2, 1], [2, 3]);
// => [1, 3]
```

### _.xorBy(arrays, iteratee=_.identity)

这种方法就像`_.xor`，不同的是它接受`iteratee`为每个元素调用每个元素`arrays`来生成他们所比较的标准。结果值的顺序由它们在数组中出现的顺序决定。迭代器被调用一个参数：*（value）*。

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Array）*：返回新的过滤值数组。

##### 例

```javascript
_.xorBy([2.1, 1.2], [2.3, 3.4], Math.floor);
// => [1.2, 3.4]
 
// The `_.property` iteratee shorthand.
_.xorBy([{ 'x': 1 }], [{ 'x': 2 }, { 'x': 1 }], 'x');
// => [{ 'x': 2 }]
```

### _.xorWith(arrays, comparator)

这个方法类似于`_.xor`，不同的是它接受`comparator`哪个被调用来比较元素`arrays`。结果值的顺序由它们在数组中出现的顺序决定。比较器被调用两个参数：*（arrVal，othVal）*。

##### 初始

4.0.0

##### 参数

1. `[arrays]` *（... Array）*：要检查的数组。
2. `[comparator]` *（Function）*：每个元素调用比较器。

##### 返回

*（Array）*：返回新的过滤值数组。

##### 例

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];

var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
 
_.xorWith(objects, others, _.isEqual);
// => [{ 'x': 2, 'y': 1 }, { 'x': 1, 'y': 1 }]
```

### _.zip(arrays)

创建一个分组元素数组，其中第一个元素包含给定数组的第一个元素，第二个元素包含给定数组的第二个元素，依此类推。

##### 初始

0.1.0

##### 参数

1. `[arrays]` *（... Array）*：要处理的数组。

##### 返回

*（Array）*：返回分组元素的新数组。

##### 例

```javascript
_.zip(['a', 'b'], [1, 2], [true, false]);
// => [['a', 1, true], ['b', 2, false]]
```

### _.zipObject([props=[]], [values=[]])

这个方法就像`_.fromPairs`，它接受两个数组，一个属性标识符和一个相应的值。

##### 初始

0.4.0

##### 参数

1. `[props=[]]` *（Array）*：属性标识符。
2. `[values=[]]` *（Array）*：属性值。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
_.zipObject(['a', 'b'], [1, 2]);
// => { 'a': 1, 'b': 2 }
```

### _.zipObjectDeep([props=[]], [values=[]])

这种方法就像`_.zipObject,`它支持属性路径。

##### 初始

4.1.0

##### 参数

1. `[props=[]]` *（Array）*：属性标识符。
2. `[values=[]]` *（Array）*：属性值。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
_.zipObjectDeep(['a.b[0].c', 'a.b[1].d'], [1, 2]);
// => { 'a': { 'b': [{ 'c': 1 }, { 'd': 2 }] } }
```

### _.zipWith(arrays, iteratee=_.identity)

这个方法就像`_.zip`,它接受`iteratee`指定应该如何组合分组值。迭代器用每个组的元素调用：*（... group）*。

##### 初始

3.8.0

##### 参数

1. `[arrays]` *（... Array）*：要处理的数组。
2. `[iteratee=_.identity]` *（Function）*：组合分组值的功能。

##### 返回

*（Array）*：返回分组元素的新数组。

##### 例

```javascript
_.zipWith([1, 2], [10, 20], [100, 200], function(a, b, c) {
  return a + b + c;
});
// => [111, 222]
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07