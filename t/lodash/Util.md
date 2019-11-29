# Util

### _.attempt(func, args)

尝试调用`func`，返回结果或捕获的错误对象。`func`调用时会提供任何其他参数。

##### 版本

3.0.0

##### 参数

1. `func` *（功能）*：尝试的功能。
2. `[args]` *（... \*）*：用来调用的参数`func`。

##### 返回

*（\*）*：返回`func`结果或错误对象。

##### 例

```javascript
// Avoid throwing errors for invalid selectors.

var elements = _.attempt(function(selector) {
  return document.querySelectorAll(selector);
}, '>_>');
 
if (_.isError(elements)) {
  elements = [];
}
```

### _.bindAll(object, methodNames)

将对象的方法绑定到对象本身，覆盖现有的方法。

**注意：**此方法不会设置绑定函数的“length”属性。

##### 以来

0.1.0

##### 参数

1. `object` *（Object）*：绑定的对象并将绑定的方法分配给。
2. `methodNames` *（...（string | string []））*：要绑定的对象方法名称。

##### Returns

*（对象）*：返回`object`。

##### 例

```javascript
var view = {
  'label': 'docs',

  'click': function() {
    console.log('clicked ' + this.label);
  }
};
 
_.bindAll(view, ['click']);

jQuery(element).on('click', view.click);
// => Logs 'clicked docs' when clicked.
```

### _.cond(pairs)

创建一个迭代`pairs`并调用第一个谓词的相应函数来返回truthy的函数。谓词函数对`this`通过创建的函数的绑定和参数来调用。

##### 以来

4.0.0

##### 参数

1. `pairs` *（数组）*：谓词函数对。

##### 返回

*（功能）*：返回新的复合功能。

##### 例

```javascript
var func = _.cond([
  [_.matches({ 'a': 1 }),           _.constant('matches A')],

  [_.conforms({ 'b': _.isNumber }), _.constant('matches B')],

  [_.stubTrue,                      _.constant('no match')]
]);
 

func({ 'a': 1, 'b': 2 });
// => 'matches A'
 

func({ 'a': 0, 'b': 1 });
// => 'matches B'
 

func({ 'a': '1', 'b': '2' });
// => 'no match'
```

### _.conforms(source)

创建一个函数，该函数`source`使用给定对象的相应属性值调用谓词属性，`true`如果所有谓词都返回truthy，则返回else `false`。

**注意：**所创建的功能相当于`_.conformsTo` 与`source`部分地施加。

##### 以来

4.0.0

##### 参数

1. `source` *（Object）*：属性谓词符合的对象。

##### 返回

*（功能）*：返回新的规格功能。

##### 例

```javascript
var objects = [
  { 'a': 2, 'b': 1 },

  { 'a': 1, 'b': 2 }
];
 
_.filter(objects, _.conforms({ 'b': function(n) { return n > 1; } }));
// => [{ 'a': 1, 'b': 2 }]
```

### _.constant(value)

创建一个返回的函数`value`。

##### 以来

2.4.0

##### 参数

1. `value` *（\*）*：从新函数返回的值。

##### 返回

*（函数）*：返回新的常量函数。

##### 例

```javascript
var objects = _.times(2, _.constant({ 'a': 1 }));
 

console.log(objects);
// => [{ 'a': 1 }, { 'a': 1 }]
 

console.log(objects[0] === objects[1]);
// => true
```

### _.defaultTo(value, defaultValue)

检查`value`以确定是否应该在其位置返回默认值。将`defaultValue` 返回如果`value`是`NaN`，`null`或`undefined`。

##### 以来

4.14.0

##### 参数

1. `value` *（\*）*：要检查的值。
2. `defaultValue` *（\*）*：默认值。

##### 返回

*（\*）*：返回已解析的值。

##### 例

```javascript
_.defaultTo(1, 10);
// => 1
 
_.defaultTo(undefined, 10);
// => 10
```

### _.flow(funcs)

创建一个函数，该函数返回调用给定函数的结果`this`并创建函数的绑定，其中每个连续调用都提供前一个函数的返回值。

##### 以来

3.0.0

##### 参数

1. `[funcs]` *（...（Function | Function []））*：要调用的函数。

##### 返回

*（功能）*：返回新的复合功能。

##### 例

```javascript
function square(n) {
  return n * n;
}
 

var addSquare = _.flow([_.add, square]);

addSquare(1, 2);
// => 9
```

### _.flowRight(funcs)

这个方法就像`_.flow`是它创建一个从右到左调用给定函数的函数。

##### 以来

3.0.0

##### 参数

1. `[funcs]` *（...（Function | Function []））*：要调用的函数。

##### 返回

*（功能）*：返回新的复合功能。

##### 例

```javascript
function square(n) {
  return n * n;
}
 

var addSquare = _.flowRight([square, _.add]);

addSquare(1, 2);
// => 9
```

### _.identity(value)

这个方法返回它接收到的第一个参数。

##### 以来

0.1.0

##### 参数

1. `value` *（\*）*：任何值。

##### 返回

*（\*）*：返回`value`。

##### 例

```javascript
var object = { 'a': 1 };
 

console.log(_.identity(object) === object);
// => true
```

### _.iteratee(func=_.identity)

创建一个调用`func`所创建函数的参数的函数。如果`func`是属性名称，则创建的函数将返回给定元素的属性值。如果`func`是数组或对象，则创建的函数将返回`true`包含等效源属性的元素，否则返回`false`。

##### 以来

4.0.0

##### 参数

1. `[func=_.identity]` *（\*）*：转换为回调的值。

##### 返回

*（功能）*：返回回调。

##### 例

```javascript
var users = [
  { 'user': 'barney', 'age': 36, 'active': true },

  { 'user': 'fred',   'age': 40, 'active': false }
];
 
// The `_.matches` iteratee shorthand.
_.filter(users, _.iteratee({ 'user': 'barney', 'active': true }));
// => [{ 'user': 'barney', 'age': 36, 'active': true }]
 
// The `_.matchesProperty` iteratee shorthand.
_.filter(users, _.iteratee(['user', 'fred']));
// => [{ 'user': 'fred', 'age': 40 }]
 
// The `_.property` iteratee shorthand.
_.map(users, _.iteratee('user'));
// => ['barney', 'fred']
 
// Create custom iteratee shorthands.
_.iteratee = _.wrap(_.iteratee, function(iteratee, func) {
  return !_.isRegExp(func) ? iteratee(func) : function(string) {
    return func.test(string);
  };
});
 
_.filter(['abc', 'def'], /ef/);
// => ['def']
```

### _.matches(source)

创建一个函数，用于在给定对象之间执行部分深层比较`source`， `true`如果给定对象具有等效属性值，则返回其他值 `false`。

**注意：**所创建的功能相当于`_.isMatch` 与`source`部分地施加。

部分比较将分别将空数组和空对象`source`值与任何数组或对象值匹配 。请参阅`_.isEqual`支持的值比较列表。

##### 以来

3.0.0

##### 参数

1. `source` *（Object）*：要匹配的属性值的对象。

##### 返回

*（功能）*：返回新的规格功能。

##### 例

```javascript
var objects = [
  { 'a': 1, 'b': 2, 'c': 3 },

  { 'a': 4, 'b': 5, 'c': 6 }
];
 
_.filter(objects, _.matches({ 'a': 4, 'c': 6 }));
// => [{ 'a': 4, 'b': 5, 'c': 6 }]
```

### _.matchesProperty(path, srcValue)

创建一个函数，用于在`path`给定对象的at值处执行部分深层比较`srcValue`，`true`如果对象值相等，则返回else `false`。

**注：**部分比较将分别将空数组和空对象`srcValue` 值与任何数组或对象值匹配。请参阅`_.isEqual`支持的值比较列表。

##### 以来

3.2.0

##### 参数

1. `path` *（Array | string）*：要获取的属性的路径。
2. `srcValue` *（\*）*：要匹配的值。

##### 返回

*（功能）*：返回新的规格功能。

##### 例

```javascript
var objects = [
  { 'a': 1, 'b': 2, 'c': 3 },

  { 'a': 4, 'b': 5, 'c': 6 }
];
 
_.find(objects, _.matchesProperty('a', 4));
// => { 'a': 4, 'b': 5, 'c': 6 }
```

### _.method(path, args)

创建一个调用`path`给定对象的方法的函数。任何额外的参数被提供给被调用的方法。

##### 以来

3.7.0

##### 参数

1. `path` *（Array | string）*：要调用的方法的路径。
2. `[args]` *（... \*）*：用来调用方法的参数。

##### 返回

*（函数）*：返回新的调用函数。

##### 例

```javascript
var objects = [
  { 'a': { 'b': _.constant(2) } },

  { 'a': { 'b': _.constant(1) } }
];
 
_.map(objects, _.method('a.b'));
// => [2, 1]
 
_.map(objects, _.method(['a', 'b']));
// => [2, 1]
```

### _.methodOf(object, args)

与之相反`_.method`; 这个方法创建一个函数调用给定路径的方法`object`。任何额外的参数被提供给被调用的方法。

##### 以来

3.7.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `[args]` *（... \*）*：用来调用方法的参数。

##### 返回

*（函数）*：返回新的调用函数。

##### 例

```javascript
var array = _.times(3, _.constant),

    object = { 'a': array, 'b': array, 'c': array };
 
_.map(['a[2]', 'c[0]'], _.methodOf(object));
// => [2, 0]
 
_.map([['a', '2'], ['c', '0']], _.methodOf(object));
// => [2, 0]
```

### _.mixin(object=lodash, source, options={})

将源对象的所有可枚举字符串键控函数属性添加到目标对象。如果`object` 是一个函数，那么方法也会添加到它的原型中。

**注意：**使用`_.runInContext`创造一个纯净的`lodash`功能，以避免因修改原始的冲突。

##### 以来

0.1.0

##### 参数

1. `[object=lodash]` *（函数|对象）*：目标对象。
2. `source` *（Object）*：要添加的函数的对象。
3. `[options={}]` *（对象）*：选项对象。
4. `[options.chain=true]` *（boolean）*：指定mixin是否可链接。

##### 返回

*（\*）：返回*`object`。

##### 例

```javascript
function vowels(string) {
  return _.filter(string, function(v) {
    return /[aeiou]/i.test(v);
  });
}
 
_.mixin({ 'vowels': vowels });
_.vowels('fred');
// => ['e']
 

_('fred').vowels().value();
// => ['e']
 
_.mixin({ 'vowels': vowels }, { 'chain': false });

_('fred').vowels();
// => ['e']
```

### _.noConflict()

将`_`变量恢复为其先前的值并返回对该`lodash` 函数的引用。

##### 以来

0.1.0

##### 返回

*（功能）*：返回`lodash`功能。

##### 例

```javascript
var lodash = _.noConflict();
```

### _.noop()

此方法返回`undefined`。

##### 以来

2.3.0

##### 例

```javascript
_.times(2, _.noop);
// => [undefined, undefined]
```

### _.nthArg(n=0)

创建一个在索引处获取参数的函数`n`。如果`n`是负数，则返回从结尾开始的第n个参数。

##### 以来

4.0.0

##### 参数

1. `[n=0]` *（数字）*：返回参数的索引。

##### 返回

*（功能）*：返回新的传递函数。

##### 例

```javascript
var func = _.nthArg(1);

func('a', 'b', 'c', 'd');
// => 'b'
 

var func = _.nthArg(-2);

func('a', 'b', 'c', 'd');
// => 'c'
```

### _.over([iteratees=_.identity])

创建一个函数，`iteratees`用它接收的参数调用并返回它们的结果。

##### 以来

4.0.0

##### 参数

1. `[iteratees=[_.identity]]` *（...（Function | Function []））*：要调用的迭代。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var func = _.over([Math.max, Math.min]);
 

func(1, 2, 3, 4);
// => [4, 1]
```

### _.overEvery([predicates=_.identity])

如果创建一个检查函数**所有**的的`predicates`时候用它接收到的参数调用返回truthy。

##### 以来

4.0.0

##### 参数

1. `[predicates=[_.identity]]` *（...（Function | Function []））*：要检查的谓词。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var func = _.overEvery([Boolean, isFinite]);
 

func('1');
// => true
 

func(null);
// => false
 

func(NaN);
// => false
```

### _.overSome([predicates=_.identity])

如果创建一个检查功能**的任何**的的`predicates`时候用它接收到的参数调用返回truthy。

##### 以来

4.0.0

##### 参数

1. `[predicates=[_.identity]]` *（...（Function | Function []））*：要检查的谓词。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var func = _.overSome([Boolean, isFinite]);
 

func('1');
// => true
 

func(null);
// => true
 

func(NaN);
// => false
```

### _.property(path)

创建一个返回`path`给定对象的值的函数。

##### 以来

2.4.0

##### 参数

1. `path` *（Array | string）*：要获取的属性的路径。

##### 返回

*（函数）*：返回新的存取函数。

##### 例

```javascript
var objects = [
  { 'a': { 'b': 2 } },

  { 'a': { 'b': 1 } }
];
 
_.map(objects, _.property('a.b'));
// => [2, 1]
 
_.map(_.sortBy(objects, _.property(['a', 'b'])), 'a.b');
// => [1, 2]
```

### _.propertyOf(object)

与之相反`_.property`; 这个方法创建一个函数，返回给定路径的值`object`。

##### 以来

3.0.0

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（函数）*：返回新的存取函数。

##### 例

```javascript
var array = [0, 1, 2],

    object = { 'a': array, 'b': array, 'c': array };
 
_.map(['a[2]', 'c[0]'], _.propertyOf(object));
// => [2, 0]
 
_.map([['a', '2'], ['c', '0']], _.propertyOf(object));
// => [2, 0]
```

### _.range(start=0, end, step=1)

创建一个从最高到但不包括的数字*（正数和/或负数）*。如果在没有or的情况下指定否定，则使用步骤。如果未指定，则设置为， 然后设置为。`startend-1startendstependstartstart0`

**注意：** JavaScript遵循IEEE-754标准来解决可能产生意外结果的浮点值。

##### 以来

0.1.0

##### 参数

1. `[start=0]` *（数字）*：范围的开始。
2. `end` *（数字）*：范围的结尾。
3. `[step=1]` *（数字）*：通过增加或减少的值。

##### 返回

*（数组）*：返回数字的范围。

##### 例

```javascript
_.range(4);
// => [0, 1, 2, 3]
 
_.range(-4);
// => [0, -1, -2, -3]
 
_.range(1, 5);
// => [1, 2, 3, 4]
 
_.range(0, 20, 5);
// => [0, 5, 10, 15]
 
_.range(0, -4, -1);
// => [0, -1, -2, -3]
 
_.range(1, 4, 0);
// => [1, 1, 1]
 
_.range(0);
// => []
```

### _.rangeRight(start=0, end, step=1)

这种方法就像`_.range`它以降序的方式填充值。

##### 以来

4.0.0

##### 参数

1. `[start=0]` *（数字）*：范围的开始。
2. `end` *（数字）*：范围的结尾。
3. `[step=1]` *（数字）*：通过增加或减少的值。

##### 返回

*（数组）*：返回数字的范围。

##### Example

```javascript
_.rangeRight(4);
// => [3, 2, 1, 0]
 
_.rangeRight(-4);
// => [-3, -2, -1, 0]
 
_.rangeRight(1, 5);
// => [4, 3, 2, 1]
 
_.rangeRight(0, 20, 5);
// => [15, 10, 5, 0]
 
_.rangeRight(0, -4, -1);
// => [-3, -2, -1, 0]
 
_.rangeRight(1, 4, 0);
// => [1, 1, 1]
 
_.rangeRight(0);
// => []
```

### _.runInContext(context=root)

`lodash`使用该`context`对象创建一个新的原始功能。

##### 以来

1.1.0

##### 参数

1. `[context=root]` *（Object）*：上下文对象。

##### 返回

*（功能）*：返回一个新`lodash`功能。

##### 例

```javascript
_.mixin({ 'foo': _.constant('foo') });
 

var lodash = _.runInContext();
lodash.mixin({ 'bar': lodash.constant('bar') });
 
_.isFunction(_.foo);
// => true
_.isFunction(_.bar);
// => false
 
lodash.isFunction(lodash.foo);
// => false
lodash.isFunction(lodash.bar);
// => true
 
// Create a suped-up `defer` in Node.js.

var defer = _.runInContext({ 'setTimeout': setImmediate }).defer;
```

### _.stubArray（）

此方法返回一个新的空数组。

##### 以来

4.13.0

##### 返回

*（数组）*：返回新的空数组。

##### 例

```javascript
var arrays = _.times(2, _.stubArray);
 

console.log(arrays);
// => [[], []]
 

console.log(arrays[0] === arrays[1]);
// => false
```

### _.stubFalse()

此方法返回`false`。

##### 以来

4.13.0

##### 返回

*（布尔）*：返回`false`。

##### 例

```javascript
_.times(2, _.stubFalse);
// => [false, false]
```

### _.stubObject()

此方法返回一个新的空对象。

##### 以来

4.13.0

##### 返回

*（Object）*：返回新的空对象。

##### 例

```javascript
var objects = _.times(2, _.stubObject);
 

console.log(objects);
// => [{}, {}]
 

console.log(objects[0] === objects[1]);
// => false
```

### _.stubString()

这个方法返回一个空字符串。

##### 以来

4.13.0

##### 返回

*（字符串）*：返回空字符串。

##### 例

```javascript
_.times(2, _.stubString);
// => ['', '']
```

### _.stubTrue()

此方法返回`true`。

##### 以来

4.13.0

##### 返回

*（布尔）*：返回`true`。

##### 例

```javascript
_.times(2, _.stubTrue);
// => [true, true]
```

### _.times(n, iteratee=_.identity)

调用迭代`n`次数，返回每个调用结果的数组。迭代器被调用一个参数; *（指数）*。

##### 以来

0.1.0

##### 参数

1. `n` *（数字）*：调用的次数`iteratee`。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（数组）*：返回结果数组。

##### 例

```javascript
_.times(3, String);
// => ['0', '1', '2']
 
 _.times(4, _.constant(0));
// => [0, 0, 0, 0]
```

### _.toPath(value)

转换`value`为属性路径数组。

##### 以来

4.0.0

##### 参数

1. `value` *（\*）*：要转换的值。

##### 返回

*（Array）*：返回新的属性路径数组。

##### 例

```javascript
_.toPath('a.b.c');
// => ['a', 'b', 'c']
 
_.toPath('a[0].b.c');
// => ['a', '0', 'b', 'c']
```

### _.uniqueId(prefix='')

生成一个唯一的ID。如果`prefix`给出，则ID附加到它。

##### 以来

0.1.0

##### 参数

1. `[prefix='']` *（字符串）*：用ID作为ID前缀的值。

##### 返回

*（字符串）*：返回唯一的ID。

##### 例

```javascript
_.uniqueId('contact_');
// => 'contact_104'
 
_.uniqueId();
// => '105'
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07