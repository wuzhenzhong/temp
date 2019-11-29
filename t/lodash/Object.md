# Object

### _.assign(object, sources)

[纠错](javascript:;)

将源对象的可枚举字符串键属性分配给目标对象。源对象从左到右应用。后续来源将覆盖之前来源的属性分配。

**注意：** 此方法变异`object`并且松散地基于[`Object.assign`](https://mdn.io/Object/assign)。

##### 版本

0.10.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `[sources]` *（...对象）*：源对象。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function Foo() {
  this.a = 1;
}
 

function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assign({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'c': 3 }
```

### _.assignIn(object, sources)

这个方法就像`_.assign`它遍历自己的和继承的源属性。

**注意：**此方法发生变化`object`。

##### 版本

4.0.0

##### 别名

*_.extend*

##### 参数

1. `object` *（对象）*：目标对象。
2. `[sources]` *（...对象）*：源对象。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function Foo() {
  this.a = 1;
}
 

function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assignIn({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'b': 2, 'c': 3, 'd': 4 }
```

### _.assignInWith(object, sources, customizer)

这种方法就像`_.assignIn`是接受`customizer` 调用它来产生分配的值。如果`customizer`返回`undefined`，则赋值由该方法处理。该`customizer`被调用，五个参数：*（objValue，srcValue，键，对象，源）*。

**注意：**此方法发生变化`object`。

版本

4.0.0

##### 别名

*_.extendWith*

##### 参数

1. `object` *（对象）*：目标对象。
2. `sources` *（...对象）*：源对象。
3. `[customizer]` *（功能）*：自定义指定值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function customizer(objValue, srcValue) {
  return _.isUndefined(objValue) ? srcValue : objValue;
}
 

var defaults = _.partialRight(_.assignInWith, customizer);
 

defaults({ 'a': 1 }, { 'b': 2 }, { 'a': 3 });
// => { 'a': 1, 'b': 2 }
```

### _.assignWith(object, sources, customizer)

这种方法就像`_.assign`是接受`customizer`调用它来产生分配的值。如果`customizer`返回`undefined`，则赋值由该方法处理。该`customizer`被调用，五个参数：*（objValue，srcValue，键，对象，源）*。

**注意：**此方法发生变化`object`。

##### 以来

4.0.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `sources` *（...对象）*：源对象。
3. `[customizer]` *（功能）*：自定义指定值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function customizer(objValue, srcValue) {
  return _.isUndefined(objValue) ? srcValue : objValue;
}
 

var defaults = _.partialRight(_.assignWith, customizer);
 

defaults({ 'a': 1 }, { 'b': 2 }, { 'a': 3 });
// => { 'a': 1, 'b': 2 }
```

### _.at(object, paths)

创建对应于`paths`的值的数组`object`。

版本

1.0.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[paths]` *（...（string | string []））*：要选择的属性路径。

##### 返回

*（数组）*：返回采集的值。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 3 } }, 4] };
 
_.at(object, ['a[0].b.c', 'a[1]']);
// => [3, 4]
```

### _.create(prototype, properties)

创建一个从`prototype`对象继承的对象。如果`properties`给定一个对象，则将其自己的可枚举字符串键控属性分配给创建的对象。

版本

2.3.0

##### 参数

1. `prototype` *（Object）*：要继承的对象。
2. `[properties]` *（对象）*：要分配给对象的属性。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
function Shape() {
  this.x = 0;
  this.y = 0;
}
 

function Circle() {
  Shape.call(this);
}
 
Circle.prototype = _.create(Shape.prototype, {
  'constructor': Circle
});
 

var circle = new Circle;
circle instanceof Circle;
// => true
 
circle instanceof Shape;
// => true
```

### _.defaults(object, sources)

将源对象的自身和继承的可枚举字符串键控属性分配给所有要解析为的目标属性的目标对象`undefined`。源对象从左到右应用。一旦设置了属性，相同属性的其他值将被忽略。

**注意：**此方法发生变化`object`。

##### 版本

0.1.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `[sources]` *（...对象）*：源对象。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
_.defaults({ 'a': 1 }, { 'b': 2 }, { 'a': 3 });
// => { 'a': 1, 'b': 2 }
```

### _.defaultsDeep(object, sources)

此方法与`_.defaults`递归分配默认属性不同。

**注意：**此方法发生变化`object`。

##### 以来

3.10.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `[sources]` *（...对象）*：源对象。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
_.defaultsDeep({ 'a': { 'b': 2 } }, { 'a': { 'b': 1, 'c': 3 } });
// => { 'a': { 'b': 2, 'c': 3 } }
```

### _.findKey(object, predicate=_.identity)

这个方法就像`_.find`是它返回第一个元素的键 `predicate`返回truthy而不是元素本身。

##### 版本

1.1.0

##### 参数

1. `object` *（对象）*：要检查的对象。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（\*）*：返回匹配元素的关键字else `undefined`。

##### 例

```javascript
var users = {
  'barney':  { 'age': 36, 'active': true },

  'fred':    { 'age': 40, 'active': false },

  'pebbles': { 'age': 1,  'active': true }
};
 
_.findKey(users, function(o) { return o.age < 40; });
// => 'barney' (iteration order is not guaranteed)
 
// The `_.matches` iteratee shorthand.
_.findKey(users, { 'age': 1, 'active': true });
// => 'pebbles'
 
// The `_.matchesProperty` iteratee shorthand.
_.findKey(users, ['active', false]);
// => 'fred'
 
// The `_.property` iteratee shorthand.
_.findKey(users, 'active');
// => 'barney'
```

### _.findLastKey(object, predicate=_.identity)

这个方法就像`_.findKey`是它以相反的顺序遍历一个集合的元素。

##### 版本

2.0.0

##### 参数

1. `object` *（对象）*：要检查的对象。
2. `[predicate=_.identity]` *（Function）*：每次迭代调用的函数。

##### Returns

*（\*）*：返回匹配元素的关键字else `undefined`。

##### 例

```javascript
var users = {
  'barney':  { 'age': 36, 'active': true },

  'fred':    { 'age': 40, 'active': false },

  'pebbles': { 'age': 1,  'active': true }
};
 
_.findLastKey(users, function(o) { return o.age < 40; });
// => returns 'pebbles' assuming `_.findKey` returns 'barney'
 
// The `_.matches` iteratee shorthand.
_.findLastKey(users, { 'age': 36, 'active': true });
// => 'barney'
 
// The `_.matchesProperty` iteratee shorthand.
_.findLastKey(users, ['active', false]);
// => 'fred'
 
// The `_.property` iteratee shorthand.
_.findLastKey(users, 'active');
// => 'pebbles'
```

### _.forIn(object, iteratee=_.identity)

迭代对象的自己和继承的可枚举字符串键控属性，并`iteratee`为每个属性调用。迭代器被调用三个参数：*（value，key，object）*。迭代器函数可以通过显式返回提前退出迭代`false`。

##### 版本

0.3.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.forIn(new Foo, function(value, key) {
  console.log(key);
});
// => Logs 'a', 'b', then 'c' (iteration order is not guaranteed).
```

### _.forInRight(object, iteratee=_.identity)

这个方法就像`_.forIn`是它`object`以相反的顺序迭代属性 。

##### 版本

2.0.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*(Object)*: Returns `object`.

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.forInRight(new Foo, function(value, key) {
  console.log(key);
});
// => Logs 'c', 'b', then 'a' assuming `_.forIn` logs 'a', 'b', then 'c'.
```

### _.forOwn(object, iteratee=_.identity)

迭代对象的自身枚举字符串键控属性并`iteratee`为每个属性调用。迭代器被调用三个参数：*（value，key，object）*。迭代器函数可以通过显式返回提前退出迭代`false`。

##### 版本

0.3.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.forOwn(new Foo, function(value, key) {
  console.log(key);
});
// => Logs 'a' then 'b' (iteration order is not guaranteed).
```

### _.forOwnRight(object, iteratee=_.identity)

这个方法就像`_.forOwn`是它`object` 以相反的顺序迭代属性。

##### 版本

2.0.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.forOwnRight(new Foo, function(value, key) {
  console.log(key);
});
// => Logs 'b' then 'a' assuming `_.forOwn` logs 'a' then 'b'.
```

### _.functions(object)

从自己的可枚举属性中创建一个函数属性名称数组`object`。

##### 版本

0.1.0

##### 参数

1. `object` *（对象）*：要检查的对象。

##### 返回

*（数组）*：返回函数名称。

##### 示例

```javascript
function Foo() {
  this.a = _.constant('a');
  this.b = _.constant('b');
}
 
Foo.prototype.c = _.constant('c');
 
_.functions(new Foo);
// => ['a', 'b']
```

### _.functionsIn(object)

从自己的继承的可枚举属性创建一个函数属性名称数组`object`。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：要检查的对象。

##### 返回

*（数组）*：返回函数名称。

##### 例

```javascript
function Foo() {
  this.a = _.constant('a');
  this.b = _.constant('b');
}
 
Foo.prototype.c = _.constant('c');
 
_.functionsIn(new Foo);
// => ['a', 'b', 'c']
```

### _.get(object, path, defaultValue)

获取`path`of 的值`object`。如果解决的值是`undefined`， `defaultValue`则返回原位。

##### 版本

3.7.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `path` *（Array | string）*：要获取的属性的路径。
3. `[defaultValue]` *（\*）*：返回`undefined`解析值的值。

##### 返回

*（\*）*：返回已解析的值。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 3 } }] };
 
_.get(object, 'a[0].b.c');
// => 3
 
_.get(object, ['a', '0', 'b', 'c']);
// => 3
 
_.get(object, 'a.b.c', 'default');
// => 'default'
```

### _.has(object, path)

检查是否`path`是否含有`object`。

版本

0.1.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `path` *（数组|字符串）*：要检查的路径。

##### 返回

*（boolean）*：返回`true`是否`path`存在，否则`false`。

##### 例

```javascript
var object = { 'a': { 'b': 2 } };

var other = _.create({ 'a': _.create({ 'b': 2 }) });
 
_.has(object, 'a');
// => true
 
_.has(object, 'a.b');
// => true
 
_.has(object, ['a', 'b']);
// => true
 
_.has(other, 'a');
// => false
```

### _.hasIn(object, path)

检查是否`path`是直接或继承的属性`object`。

##### 版本

4.0.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `path` *（数组|字符串）*：要检查的路径。

##### 返回

*（boolean）*：返回`true`是否`path`存在，否则`false`。

##### 示例

```javascript
var object = _.create({ 'a': _.create({ 'b': 2 }) });
 
_.hasIn(object, 'a');
// => true
 
_.hasIn(object, 'a.b');
// => true
 
_.hasIn(object, ['a', 'b']);
// => true
 
_.hasIn(object, 'b');
// => false
```

### _.invert(object)

创建一个由倒置键和值组成的对象`object`。如果`object`包含重复值，则后续值将覆盖以前值的属性分配。

##### 版本

0.7.0

##### 参数

1. `object` *（对象）*：要反转的对象。

##### 返回

*（Object）*：返回新的倒置对象。

##### 例

```javascript
var object = { 'a': 1, 'b': 2, 'c': 1 };
 
_.invert(object);
// => { '1': 'c', '2': 'b' }
```

### _.invertBy(object, iteratee=_.identity)

这种方法是等`_.invert`不同之处在于，从运行中的每个元素的结果所产生的反转对象`object`通`iteratee`。每个反转键的相应反转值是负责产生反转值的键数组。迭代器被调用一个参数：*（value）*。

##### 版本

4.1.0

##### 参数

1. `object` *（对象）*：要反转的对象。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（Object）*：返回新的倒置对象。

##### 例

```javascript
var object = { 'a': 1, 'b': 2, 'c': 1 };
 
_.invertBy(object);
// => { '1': ['a', 'c'], '2': ['b'] }
 
_.invertBy(object, function(value) {
  return 'group' + value;
});
// => { 'group1': ['a', 'c'], 'group2': ['b'] }
```

### _.invoke(object, path, args)

调用`path`of 处的方法`object`。

##### 版本

4.0.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `path` *（Array | string）*：要调用的方法的路径。
3. `[args]` *（... \*）*：用来调用方法的参数。

##### 返回

*(\*)*: Returns the result of the invoked method.

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': [1, 2, 3, 4] } }] };
 
_.invoke(object, 'a[0].b.c.slice', 1, 3);
// => [2, 3]
```

### _.keys(object)

创建一个自己的可枚举属性名称的数组`object`。

**注意：** 非对象值被强制转换为对象。有关更多详细信息，请参阅[ES规范](http://ecma-international.org/ecma-262/7.0/#sec-object.keys)。

##### 版本

0.1.0

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（Array）*：返回属性名称的数组。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.keys(new Foo);
// => ['a', 'b'] (iteration order is not guaranteed)
 
_.keys('hi');
// => ['0', '1']
```

### _.keysIn(object)

创建一个自己的继承的可枚举属性名称的数组 `object`。

**注意：**非对象值被强制转换为对象。

##### 版本

3.0.0

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（Array）*：返回属性名称的数组。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.keysIn(new Foo);
// => ['a', 'b', 'c'] (iteration order is not guaranteed)
```

### _.mapKeys(object, iteratee=_.identity)

与之相反`_.mapValues`; 此方法创建一个具有与`object`通过运行`object`thru的每个自己的可枚举字符串键控属性生成的键值 相同的值的对象`iteratee`。迭代器被调用三个参数：*（value，key，object）*。

##### 版本

3.8.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Object）*：返回新的映射对象。

##### 例

```javascript
_.mapKeys({ 'a': 1, 'b': 2 }, function(value, key) {
  return key + value;
});
// => { 'a1': 1, 'b2': 2 }
```

### _.mapValues(object, iteratee=_.identity)

使用与`object`通过运行`object`thru的每个自己的可枚举字符串键控属性所生成的相同的键和值创建一个对象`iteratee`。迭代器被调用三个参数：

*(value, key, object)*.

##### 版本

2.4.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。

##### 返回

*（Object）*：返回新的映射对象。

##### 例

```javascript
var users = {
  'fred':    { 'user': 'fred',    'age': 40 },

  'pebbles': { 'user': 'pebbles', 'age': 1 }
};
 
_.mapValues(users, function(o) { return o.age; });
// => { 'fred': 40, 'pebbles': 1 } (iteration order is not guaranteed)
 
// The `_.property` iteratee shorthand.
_.mapValues(users, 'age');
// => { 'fred': 40, 'pebbles': 1 } (iteration order is not guaranteed)
```

### _.merge(object, sources)

这种方法就像`_.assign`是递归地将源对象的自己和继承的可枚举字符串键控属性合并到目标对象中。`undefined`如果目标值存在，则解析为跳过的源属性 。数组和简单对象属性是递归合并的。其他对象和值类型被赋值覆盖。源对象从左到右应用。后续来源将覆盖之前来源的属性分配。

**注意：**此方法发生变化`object`。

##### 版本

0.5.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `[sources]` *（...对象）*：源对象。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
var object = {
  'a': [{ 'b': 2 }, { 'd': 4 }]
};
 

var other = {
  'a': [{ 'c': 3 }, { 'e': 5 }]
};
 
_.merge(object, other);
// => { 'a': [{ 'b': 2, 'c': 3 }, { 'd': 4, 'e': 5 }] }
```

### _.mergeWith(object, sources, customizer)

此方法类似，`_.merge`只是它接受`customizer`调用它来生成目标和源属性的合并值。如果`customizer`返回 `undefined`，则合并由该方法处理。该`customizer`调用有六个参数：

*(objValue, srcValue, key, object, source, stack)*.

**注意：**此方法发生变化`object`。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：目标对象。
2. `sources` *（...对象）*：源对象。
3. `customizer` *（功能）*：自定义指定值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
function customizer(objValue, srcValue) {
  if (_.isArray(objValue)) {
    return objValue.concat(srcValue);
  }
}
 

var object = { 'a': [1], 'b': [2] };

var other = { 'a': [3], 'b': [4] };
 
_.mergeWith(object, other, customizer);
// => { 'a': [1, 3], 'b': [2, 4] }
```

### _.omit(object, paths)

与之相反`_.pick`; 此方法创建一个由自己和继承的枚举属性路径组成的对象，这些路径`object`不会被省略。

**注意：**这个方法比`_.pick`慢

##### 版本

0.1.0

##### 参数

1. `object` *（对象）*：源对象。
2. `[paths]` *（...（string | string []））*：要忽略的属性路径。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.omit(object, ['a', 'c']);
// => { 'b': '2' }
```

### _.omitBy(object, predicate=_.identity)

与之相反`_.pickBy`; 此方法创建自己组成的对象和继承的枚举字符串键的属性`object`是`predicate`不返回truthy的。谓词用两个参数调用：*（value，key）*。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：源对象。
2. `[predicate=_.identity]` *（Function）*：每个属性调用的函数。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.omitBy(object, _.isNumber);
// => { 'b': '2' }
```

### _.pick(object, paths)

创建一个由拾取`object`属性组成的对象。

##### 版本

0.1.0

##### 参数

1. `object` *（对象）*：源对象。
2. `[paths]` *（...（string | string []））*：要选择的属性路径。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.pick(object, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

### _.pickBy(object, predicate=_.identity)

创建一个由`object`属性`predicate`返回truthy 组成的对象。谓词用两个参数调用：*（value，key）*。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：源对象。
2. `[predicate=_.identity]` *（Function）*：每个属性调用的函数。

##### 返回

*（Object）*：返回新的对象。

##### 例

```javascript
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.pickBy(object, _.isNumber);
// => { 'a': 1, 'c': 3 }
```

### _.result(object, path, defaultValue)

这种方法就像是`_.get`，如果解析的值是一个函数，它的`this`父对象的绑定将被调用，并返回结果。

##### 版本

0.1.0

##### 参数

1. `object` *（Object）*：要查询的对象。
2. `path` *（Array | string）*：要解析的属性的路径。
3. `[defaultValue]` *（\*）*：返回`undefined`解析值的值。

##### 返回

*（\*）*：返回已解析的值。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c1': 3, 'c2': _.constant(4) } }] };
 
_.result(object, 'a[0].b.c1');
// => 3
 
_.result(object, 'a[0].b.c2');
// => 4
 
_.result(object, 'a[0].b.c3', 'default');
// => 'default'
 
_.result(object, 'a[0].b.c3', _.constant('default'));
// => 'default'
```

### _.set(object, path, value)

设置在价值`path`的`object`。如果一部分`path`不存在，则创建它。为缺少索引属性创建数组，而为所有其他缺少的属性创建对象。使用`_.setWith`自定义`path` 创建。

**注意：**此方法发生变化`object`。

##### 版本

3.7.0

##### 参数

1. `object` *（对象）*：要修改的对象。
2. `path` *（Array | string）*：要设置的属性的路径。
3. `value` *（\*）*：要设置的值。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 3 } }] };
 
_.set(object, 'a[0].b.c', 4);

console.log(object.a[0].b.c);
// => 4
 
_.set(object, ['x', '0', 'y', 'z'], 5);

console.log(object.x[0].y.z);
// => 5
```

### _.setWith(object, path, value, customizer)

这个方法类似于`_.set`它接受`customizer`被调用来产生对象的方法`path`。如果`customizer`返回`undefined`路径创建是由方法处理的。该`customizer`调用有三个参数：*（nsValue，key，nsObject）*。

**注意：**此方法发生变化`object`。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：要修改的对象。
2. `path` *（Array | string）*：要设置的属性的路径。
3. `value` *（\*）*：要设置的值。
4. `[customizer]` *（功能）*：自定义指定值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
var object = {};
 
_.setWith(object, '[0][1]', 'a', Object);
// => { '0': { '1': 'a' } }
```

### _.toPairs(object)

创建一个可`object`由其使用的自己可枚举的字符串键控值对的数组`_.fromPairs`。如果`object`是地图或集合，则返回其条目。

##### 版本

4.0.0

##### 别名

*_.entries*

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（数组）*：返回键值对。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.toPairs(new Foo);
// => [['a', 1], ['b', 2]] (iteration order is not guaranteed)
```

### _.toPairsIn(object)

创建一个自己的和继承的可枚举的字符串键值对的数组，供`object`其使用`_.fromPairs`。如果`object`是地图或集合，则返回其条目。

##### 版本

4.0.0

##### 别名

*_.entriesIn*

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（数组）*：返回键值对。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.toPairsIn(new Foo);
// => [['a', 1], ['b', 2], ['c', 3]] (iteration order is not guaranteed)
```

### _.transform(object, iteratee=_.identity, accumulator)

替代方案`_.reduce`; 这个方法转换`object`为一个新的 `accumulator`对象，这个对象是通过运行每个自己的可枚举字符串键控属性的结果`iteratee`，每个调用都可能会突变`accumulator`对象。如果`accumulator` 没有提供，`[[Prototype]]`将使用一个相同的新对象。迭代器被调用四个参数:( *累加器，值，键，对象）*。迭代器函数可以通过显式返回提前退出迭代`false`。

##### 版本

1.3.0

##### 参数

1. `object` *（Object）*：迭代的对象。
2. `[iteratee=_.identity]` *（Function）*：每次迭代调用的函数。
3. `[accumulator]` *（\*）*：自定义累加器值。

##### 返回

*（\*）*：返回累计值。

##### 例

```javascript
_.transform([2, 3, 4], function(result, n) {
  result.push(n *= n);
  return n % 2 == 0;
}, []);
// => [4, 9]
 
_.transform({ 'a': 1, 'b': 2, 'c': 1 }, function(result, value, key) {
  (result[value] || (result[value] = [])).push(key);
}, {});
// => { '1': ['a', 'c'], '2': ['b'] }
```

### _.unset(object, path)

移除`path`of 处的财产`object`。

**注意：**此方法发生变化 `object`。

##### 版本

4.0.0

##### 参数

1. `object` *（对象）*：要修改的对象。
2. `path` *（数组|字符串）*：要设置的属性的路径。

##### 返回

*（boolean）*：返回`true`属性是否被删除，否则`false`。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 7 } }] };
_.unset(object, 'a[0].b.c');
// => true
 

console.log(object);
// => { 'a': [{ 'b': {} }] };
 
_.unset(object, ['a', '0', 'b', 'c']);
// => true
 

console.log(object);
// => { 'a': [{ 'b': {} }] };
```

### _.update(object, path, updater)

这种方法就像`_.set`接受`updater`产生要设置的值一样。使用`_.updateWith`自定义`path`创建。该 `updater`被调用，一个参数：*（值）*。

**注意：**此方法发生变化`object`。

##### 版本

4.6.0

##### 参数

1. `object` *（对象）*：要修改的对象。
2. `path` *（Array | string）*：要设置的属性的路径。
3. `updater` *（功能）*：产生更新值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
var object = { 'a': [{ 'b': { 'c': 3 } }] };
 
_.update(object, 'a[0].b.c', function(n) { return n * n; });

console.log(object.a[0].b.c);
// => 9
 
_.update(object, 'x[0].y.z', function(n) { return n ? n + 1 : 0; });

console.log(object.x[0].y.z);
// => 0
```

### _.updateWith(object, path, updater, customizer)

这个方法类似于`_.update`它接受`customizer`被调用来产生对象的方法`path`。如果`customizer`返回`undefined` 路径创建是由方法处理的。该`customizer`调用有三个参数：*（nsValue，key，nsObject）*。

**注意：**此方法发生变化`object`。

##### 版本

4.6.0

##### 参数

1. `object` *（对象）*：要修改的对象。
2. `path` *（Array | string）*：要设置的属性的路径。
3. `updater` *（功能）*：产生更新值的功能。
4. `[customizer]` *（功能）*：自定义指定值的功能。

##### 返回

*（对象）*：返回`object`。

##### 例

```javascript
var object = {};
 
_.updateWith(object, '[0][1]', _.constant('a'), Object);
// => { '0': { '1': 'a' } }
```

### _.values(object)

创建一个自己的可枚举字符串键控属性值的数组`object`。

**注意：**非对象值被强制转换为对象。

##### 版本

0.1.0

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（Array）*：返回属性值的数组。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.values(new Foo);
// => [1, 2] (iteration order is not guaranteed)
 
_.values('hi');
// => ['h', 'i']
```

### _.valuesIn(object)

创建自己的和继承的可枚举字符串键控属性值的数组 `object`。

**注意：**非对象值被强制转换为对象。

##### 版本

3.0.0

##### 参数

1. `object` *（Object）*：要查询的对象。

##### 返回

*（Array）*：返回属性值的数组。

##### 例

```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.valuesIn(new Foo);
// => [1, 2, 3] (iteration order is not guaranteed)
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07